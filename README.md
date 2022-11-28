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
  * [`labels`](#labels)
  * [`tail`](#tail)
  * [`grep`](#grep)
* [ENVIRONMENT](#environment)
  * [`KUBECLI_GREP`](#kubecli_grep)
  * [`KUBECLI_NS`](#kubecli_ns)
  * [`KUBECLI_SEL`](#kubecli_sel)
  * [`KUBECLI_SUDO`](#kubecli_sudo)
* [ASSOCIATIONS AND CULTURAL REFERENCES](#associations-and-cultural-references)
* [REFERENCES](#references)
* [SEE ALSO](#see-also)
* [LICENSE](#license)
<!-- toc-end -->


# NAME

`kubecli` - extended control over Kubernetes

# SYNOPSIS

    kubecli [OPTIONS]
    kubectl [OPTIONS]

# DESCRIPTION

The `kubecli` project implements the extended management on Kubernetes.

## Requirements

* kubectl
* bash
* grep
* awk
* sed

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

Print the `kubecli` short usage:

    kubecli help


## `env`

Print any kubernetes related variables or set some of kubecli specific:

    kubecli env [-n NAMESPACE] [-l SELECTOR]

Kubernetes itself uses some environment variables which names look like `KUBE*`. This project also adds a few variables named as `KUBECLI_*`. See the details in the corresponding section below.


## `names`

List the entity names only: any of namespaces, pods, etc:

    kubecli names NAME [OPTIONS]
    kubectl get NAME [OPTIONS] -o name

Two examples above show two options how to achieve the same result with the new function and in the classical way.


## `labels`

Collect the entities and reorder them against the labels they associated:

    kubecli labels NAME [OPTIONS]
    kubectl get NAME [OPTIONS] \
        --no-headers \
        -o custom-columns=NAMES:.metadata.name,LABELS:.metadata.labels

This command can be useful to recognize labels for selectors when needs come to define the pods for logging.

Besides that this command reorders the collected list putting the labels in the front of the associated list of pods and other entities.


## `tail`

Display logs for selected entities:

    kubecli tail [SELECTOR] [OPTIONS]
    kubectl logs SELECTOR [OPTIONS]

It's definitely something useful: keep a namespace and pod selectors in `KUBECLI_NS` and `KUBECLI_SEL`, the environment variables and call this command to monitor pods' log messages.


## `grep`

Print log lines matching GREP-OPTIONS:

    kubecli grep [SELECTOR] [OPTIONS] -- GREP-OPTIONS
    kubecli tail [SELECTOR] [OPTIONS] | grep GREP-OPTIONS
    kubectl logs SELECTOR [OPTIONS] | grep GREP-OPTIONS

Take logs of pods specified by the selector and filter particular lines. If the `KUBECLI_SEL` variable is set and not empty, it is used implicitly.

It's syntactic sugar combining `kubecli tail` and `grep`. For more complex cases use `kubecli tail` with alternative grep-like tools or specify the `KUBECLI_GREP` variable.

**Note**: the `--` parameter in the first command is mandatory because it separates arguments for `kubectl` and `grep`, respectively.


# ENVIRONMENT

These variables are used by `kubecli` and `kubectl`. If one of them is declared and is not empty, it is used according to the explanations below.

## `KUBECLI_GREP`

Enables an alternative command for filtering log lines. If not specified, `grep` is used.

## `KUBECLI_NS`

Declares a namespace for invocation as `kubectl -n "$KUBECLI_NS"`.

## `KUBECLI_SEL`

Declares a selector for invocation as `kubectl logs -l "$KUBECLI_SEL"`.

## `KUBECLI_SUDO`

If required, declares the `sudo` command and optionally its parameters used to elevate privileges for `kubectl`. It can be useful when the command is executed under regular users. No needs to declare it for root.

At least one of the following examples is applicable for you (just specify the right path to the configuration file):

    KUBECLI_SUDO=sudo
    KUBECLI_SUDO='sudo -i'
    KUBECLI_SUDO='sudo -E'
    KUBECLI_SUDO='sudo -E bash -l'
    KUBECLI_SUDO='sudo --preserve-env=KUBECONFIG'
    KUBECLI_SUDO='sudo KUBECONFIG=/etc/kubernetes/admin.conf'

# ASSOCIATIONS AND CULTURAL REFERENCES

Initially, when I started developing this project, one allusion has stuck in my mind - "Kin-dza-dza!", the famous movie and masterpiece by the Soviet producer Georgy Danelia. It is funny sci-fi, a bit humoristic, a bit satirical.

Events take place partially on our Earth and mostly on Plyuk, the alien planet. The Plyukanian language has the word koo /ku/ meaning any other word. It sounds like the first sillable of the kubectl word. And nothing more.

Later I realized that kubecli is good name for the project because it is CLI tool for kube (the short form of kubectl). In the other hand, kubecli /kubekli/ sounds almost like күбәләк /kʏbæ'læk/, the Tatar word meaning butterfly.

When development became closer to finish and was almost ready for publishing I re-read some facts about "Kin-dza-dza!". Pepelats, another Plyukanian word meaning a spaceship is originated from the Georgian word პეპელა /'pepela/ that is translated as butterfly.

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

Here is collection of links to other projects similar to this one or implementing more interesting features.

* https://github.com/deniskrumko/kube-cli
* https://github.com/ahmetb/kubectx
* https://github.com/collabnix/kubetools

# LICENSE

Copyright 2022, Ildar Shaimordanov

    MIT License

