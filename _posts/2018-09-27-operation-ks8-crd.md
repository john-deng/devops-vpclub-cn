---
ID: 361
post_title: 'Kubernetes CRD &#8211; 从代码生成到使用'
post_name: operation-ks8-crd
author: 邓冰寒
post_date: 2018-09-27 12:00:00
layout: post
link: >
  https://devops.vpclub.cn/operation-ks8-crd/
published: true
tags:
  - CRD
  - k8s
categories:
  - 运维
  - 运维
---

# Kubernetes CRD - 从代码生成到使用

CustomResourceDefinition（CRD）是 v1.7 + 新增的无需改变代码就可以扩展 Kubernetes API 的机制，用来管理自定义对象。它实际上是 ThirdPartyResources（TPR） 的升级版本，而 TPR 已经在 v1.8 中删除。

一些使用场景：
提供/管理外部数据存储/数据库(例如 CloudSQL/RDS 实例)
对k8s基础资源进行更高层次的抽象(比如定义一个etcd集群)
其实crd在很多k8s周边开源项目中有使用，比如ingress-controller和众多的operator

## 1. 目录结构

```bash
pkg
    |--apis
        |--samplecontroller
            |--v1alpha1
                |--doc.go
                |--register.go
                |--types.go
            |--register.go
    |--client
```

## 2. 配置Gopkg.toml:

配置完成后执行dep ensure -v 下载依赖

```toml
required = [
  "k8s.io/code-generator/cmd/client-gen"
]

[[constraint]]
  name = "k8s.io/client-go"
  version = "v6.0.0"

[[constraint]]
  name = "k8s.io/api"
  version = "kubernetes-1.10.4"

[[constraint]]
  name = "k8s.io/apimachinery"
  version = "kubernetes-1.10.4"

[[constraint]]
  name = "k8s.io/code-generator"
  version = "kubernetes-1.10.4"

[prune]
  non-go = true
  go-tests = true
  unused-packages = true

  [[prune.project]]
    name = "k8s.io/code-generator"
    unused-packages = false
    non-go = false
    go-tests = false

  [[prune.project]]
    name = "k8s.io/gengo"
    unused-packages = false
    non-go = false
    go-tests = false
```

## 3. 生成crd客户端代码

### 1.1 先编写 doc.go ，types.go ,register.go 代码

doc.go

```go
// +k8s:deepcopy-gen=package

// Package v1alpha1 is the v1alpha1 version of the API.
// +groupName=samplecontroller.k8s.io
package v1alpha1
```

types.go

```go
package v1alpha1

import (
  metav1 "k8s.io/apimachinery/pkg/apis/meta/v1"  
)

// +genclient
// +k8s:deepcopy-gen:interfaces=k8s.io/apimachinery/pkg/runtime.Object

// Foo is a specification for a Foo resource
type Foo struct {
	metav1.TypeMeta   `json:",inline"`
	metav1.ObjectMeta `json:"metadata,omitempty"`

	Spec   FooSpec   `json:"spec"`
	Status FooStatus `json:"status"`
}



// FooSpec is the spec for a Foo resource
type FooSpec struct {
	DeploymentName string `json:"deploymentName"`
	Replicas       *int32 `json:"replicas"`
}

// FooStatus is the status for a Foo resource
type FooStatus struct {
	AvailableReplicas int32 `json:"availableReplicas"`
}

// +k8s:deepcopy-gen:interfaces=k8s.io/apimachinery/pkg/runtime.Object

// FooList is a list of Foo resources
type FooList struct {
	metav1.TypeMeta `json:",inline"`
	metav1.ListMeta `json:"metadata"`

	Items []Foo `json:"items"`
}
```

register.go

```go
package v1alpha1

import (
	metav1 "k8s.io/apimachinery/pkg/apis/meta/v1"
	"k8s.io/apimachinery/pkg/runtime"
	"k8s.io/apimachinery/pkg/runtime/schema"

	samplecontroller "github.com/hidevopsio/hioak/pkg/apis/samplecontroller"
)

// SchemeGroupVersion is group version used to register these objects
var SchemeGroupVersion = schema.GroupVersion{Group: samplecontroller.GroupName, Version: "v1alpha1"}

// Kind takes an unqualified kind and returns back a Group qualified GroupKind
func Kind(kind string) schema.GroupKind {
	return SchemeGroupVersion.WithKind(kind).GroupKind()
}

// Resource takes an unqualified resource and returns a Group qualified GroupResource
func Resource(resource string) schema.GroupResource {
	return SchemeGroupVersion.WithResource(resource).GroupResource()
}

var (
	SchemeBuilder = runtime.NewSchemeBuilder(addKnownTypes)
	AddToScheme   = SchemeBuilder.AddToScheme
)

// Adds the list of known types to Scheme.
func addKnownTypes(scheme *runtime.Scheme) error {
	scheme.AddKnownTypes(SchemeGroupVersion,
		&Foo{},
		&FooList{},
	)
	metav1.AddToGroupVersion(scheme, SchemeGroupVersion)
	return nil
}

```

register.go(此文件是在samplecontroller目录下)
```
package samplecontroller

const (
	GroupName = "samplecontroller.k8s.io"
)
```

### 1.2 执行生成代码的命令

- 1.进入project目录  $cd $GOPATH/src/github.com/openshift-evangelists/crd-code-generation/vendor/k8s.io/code-generator
- 2.给generate-groups.sh 添加执行权限  $chmod +x generate-groups.sh
- 3.在当前目录执行以下命令

```bash
./generate-groups.sh all \
github.com/hidevopsio/hioak/pkg/client \
github.com/hidevopsio/hioak/pkg/apis \
samplecontroller:v1alpha1
```

四个参数：

- 第一个 参数：all, 是说，要生成所有的模块，如clientset,informers,listers等
- 第二个参数：github.com/hidevopsio/hioak/pkg/client  这个是你要生成代码的目录，目录的名称是client, 也有叫generated的
- 第三个参数：github.com/hidevopsio/hioak/pkg/apis  这个目录就是你自己创建的，里面至少包括types.go那个目录
- 第四个参数："samplecontroller:v1alpha1"： 这个就是目录，samplecontroller是apis下的目录，v1beta1是samplecontroller下面的目录

运行generate-groups.sh后，会在gopath目录的bin目录下，生成5个工具，如client-gen,deepcopy-gen等
就是利用这几个工具，来生成代码的

生成代码可能遇到的问题:

```bash
1.提示 ./generate-groups.sh: line 65: /Users/mac/.gvm/pkgsets/go1.10/vpcloud:/Users/mac/.gvm/pkgsets/go1.10/global/bin/deepcopy-gen: No such file or directory
原因是我的GOPATH有两个目录，脚本默认是获取GOPATH对应的字符串未路径
解决方法:export GOPATH=/Users/mac/.gvm/pkgsets/go1.10/vpcloud
如果还是此类报错就要按照提示进入deepcopy-gen所在目录，查看deepcopy-gen是否存在并执行deepcopy-gen 来分析更加精准的异常
```

### 1.3代码生成后的目录结构

```bash
pkg
  |--apis
        |--samplecontroller
            |--v1alpha1
                |--doc.go
                |--register.go
                |--types.go
                |--zz_generated.deepcopy.go
            |--register.go
  |--client
        |--clientset
        |--informers
        |--listers
```

## 4. 使用生成的代码

### 4.1 创建FOO类型的crd及使用

kubectl create -f crd.yaml

crd.yaml:

```yaml
apiVersion: apiextensions.k8s.io/v1beta1
kind: CustomResourceDefinition
metadata:
  name: foos.samplecontroller.k8s.io
spec:
  group: samplecontroller.k8s.io
  version: v1alpha1
  names:
    kind: Foo
    plural: foos
  scope: Namespaced
```

kubectl create -f example-foo.yaml

example-foo.yaml:

```yaml
apiVersion: samplecontroller.k8s.io/v1alpha1
kind: Foo
metadata:
  name: example-foo
spec:
  deploymentName: example-foo
  replicas: 1
```

### 4.2 创建一个main.go 文件直接使用

main.go

```go

package main

import (
	"flag"
	"fmt"
	"github.com/golang/glog"
	clientset "hidevops.io/sample-controller/pkg/client/clientset/versioned"
	"k8s.io/client-go/tools/clientcmd"
	metav1 "k8s.io/apimachinery/pkg/apis/meta/v1"
)

var (
	masterURL  string
	kubeconfig string
)

func init() {
	flag.StringVar(&kubeconfig, "kubeconfig", "/Users/mac/.kube/config", "Path to a kubeconfig. Only required if out-of-cluster.")
	flag.StringVar(&masterURL, "master", "", "The address of the Kubernetes API server. Overrides any value in kubeconfig. Only required if out-of-cluster.")
}

func main() {
	flag.Parse()

	cfg, err := clientcmd.BuildConfigFromFlags(masterURL, kubeconfig)
	if err != nil {
		glog.Fatalf("Error building kubeconfig: %s", err.Error())
	}

	exampleClient, err := clientset.NewForConfig(cfg)
	if err != nil {
		glog.Fatalf("Error building example clientset: %s", err.Error())
	}

	fooList,err := exampleClient.SamplecontrollerV1alpha1().Foos("defaule").List(metav1.ListOptions{})
	if err != nil {
		fmt.Printf("Error %v",err)
		return
	}
	fmt.Println("fooList ",fooList)

}

```

参考资料:

1. https://blog.openshift.com/kubernetes-deep-dive-code-generation-customresources/

2. https://cloud.tencent.com/info/b017df5e65dad32a161580fd1affe3d1.html

3. https://github.com/kubernetes/sample-controller
