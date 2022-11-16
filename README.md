<!-- toc-begin -->
# Table of Content
* [NAME](#name)
* [SYNOPSIS](#synopsis)
* [DESCRIPTION](#description)
  * [Requirements](#requirements)
  * [Installation](#installation)
  * [`kubectl`](#kubectl)
  * [`kubecli`](#kubecli)
* [COMMANDS](#commands)
  * [`help`](#help)
  * [`env`](#env)
  * [`names`](#names)
  * [`find`](#find)
  * [`tail`](#tail)
  * [`grep`](#grep)
* [ENVIRONMENT](#environment)
  * [`KUBECLI_NS`](#kubecli_ns)
  * [`KUBECLI_SEL`](#kubecli_sel)
* [ASSOCIATIONS AND CULTURAL REFERENCES](#associations-and-cultural-references)
* [REFERENCES](#references)
* [SEE ALSO](#see-also)
* [LICENSE](#license)
<!-- toc-end -->

# NAME

`kubecli` - extended control over Kubernetes

# SYNOPSIS

    kubecli ...
    kubectl ...

# DESCRIPTION

The `kubecli` project implements the extended management on Kubernetes.

## Requirements

* kubectl
* bash
* grep

## Installation

Download the `.kubecli` file from this repository and put it in the home directory. After that source it in `.bashrc` or in shell scripts where you'd like to add its functionality.

    . ~/.kubecli

## `kubectl`

It's wrapper function around the `kubectl` executable itself and possibly other launchers like `minikube kubectl`, `k3s kubectl` and `rancher kubectl`.

Other forms have been taken into account also because they are mentioned in the official documentations of each distribution.

## `kubecli`

It's the real workhorse. It does everything what `kubectl` is able to do and also does a bit more. The following section describes extended commands supported by `kubecli`.

A command is passed as the first argument. If the argument is recognized as the `kubecli` extended command, it is processed internally. Otherwise everything is passed to `kubectl`.

# COMMANDS

## `help`

Print `kubecli` usage:

    kubecli help

## `env`

Print Kubernetes related environment variables:

    kubecli env

Kubernetes itself uses some environment variables which names look like `KUBE*`. This project adds a few variables named as `KUBECLI_*`. Details are in the corresponding section below.

## `names`

List the entity names only: any of namespaces, pods, etc:

    kubecli names NAME ...
    kubectl get NAME ... -o name

Two examples above show two options how to achieve the same result with the new function and in the classical way.

## `find`

Find the required entities with names matching GREP-OPTIONS:

    kubecli find NAME ... -- GREP-OPTIONS
    kubecli names NAME ... | grep GREP-OPTIONS
    kubectl get NAME ... -o name | grep GREP-OPTIONS

In fact, the first command above is a light improvement for the second command. And in the same time it is syntactic sugar for the third classical command.

**Note**: the `--` parameter in the first command is mandatory because it separates arguments for `kubectl` and `grep`, respectively.

## `tail`

Display logs for selected entities:

    kubecli tail SELECTOR ...
    kubectl logs SELECTOR ...

It's definitely something very useful: keep a namespace and pod selectors in environment variables and call a command like this to monitor pods' log messages.

## `grep`

Print log lines matching GREP-OPTIONS:

    kubecli grep SELECTOR ... -- GREP-OPTIONS
    kubecli tail SELECTOR ... | grep GREP-OPTIONS
    kubectl logs SELECTOR ... | grep GREP-OPTIONS

It's syntactic sugar for the previous command. More frequently we need to filter some particular lines in logs. For more complex cases use `kubecli tail` in combination with alternative grep-like tools.

**Note**: the `--` parameter in the first command is mandatory because it separates arguments for `kubectl` and `grep`, respectively.

# ENVIRONMENT

These variables are used by `kubecli`. If one of them is declared and is not empty, it is used according to the explanations below.

## `KUBECLI_NS`

Declares a namespace for invocation as `kubectl -n "$KUBECLI_NS"`.

## `KUBECLI_SEL`

Declares a namespace for invocation as `kubectl -l "$KUBECLI_SEL" log`.

# ASSOCIATIONS AND CULTURAL REFERENCES

Initially, when I started developing this project, one allusion has stuck in my mind - "Kin-dza-dza!", the famous movie and masterpiece by the Soviet producer Georgy Danelia. It is funny sci-fi, a bit humoristic, a bit satirical.

Events take place partially on our Earth and mostly on Plyuk, the alien planet. The Plyukanian language has the word koo /ku/ meaning any other word. It sounds like the first sillable of the kubectl word. And nothing more.

Later I realized that kubecli is good name for the project because it is CLI tool for kube (the short form of kubectl). In the other hand, kubecli /kubekli/ sounds almost like күбәләк /kʏbæ'læk/, the Tatar word meaning butterfly.

When development was becoming closer to finish and was almost ready for publishing I re-read some facts about "Kin-dza-dza!". Another Plyukanian word pepelats (a spaceship) is originated from the Georgian word პეპელა /'pepela/ that is translated as butterfly.

So, the circle has closed! Koo!

# REFERENCES

Kubectl commands

* https://kubernetes.io/docs/reference/kubectl/kubectl/
* https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands

kubectl Cheat Sheet

* https://kubernetes.io/docs/reference/kubectl/cheatsheet/

Custom columns

* https://kubernetes.io/docs/reference/kubectl/#custom-columns

JSONPath Support

* https://kubernetes.io/docs/reference/kubectl/jsonpath/

# SEE ALSO

This section is collection of links to other projects implementing something similar this or more interesting features.

* https://github.com/deniskrumko/kube-cli/

# LICENSE

Copyright 2022, Ildar Shaimordanov

    MIT License
