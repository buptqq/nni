概述
========

NNI (Neural Network Intelligence) 是一个工具包，可有效的帮助用户设计并调优机器学习模型的神经网络架构，复杂系统的参数（如超参）等。 NNI 的特性包括：易于使用，可扩展，灵活，高效。


* **易于使用**：NNI 可通过 pip 安装。 只需要在代码中添加几行，就可以利用 NNI 来调优参数。 可使用命令行工具或 Web 界面来查看 Experiment。
* **可扩展**：调优超参或网络结构通常需要大量的计算资源。NNI 在设计时就支持了多种不同的计算资源，如远程服务器组，训练平台（如：OpenPAI，Kubernetes），等等。 根据您配置的培训平台的能力，可以并行运行数百个 Trial 。
* **灵活**：除了内置的算法，NNI 中还可以轻松集成自定义的超参调优算法，神经网络架构搜索算法，提前终止算法等等。 还可以将 NNI 连接到更多的训练平台上，如云中的虚拟机集群，Kubernetes 服务等等。 此外，NNI 还可以连接到外部环境中的特殊应用和模型上。
* **高效**：NNI 在系统及算法级别上不断地进行优化。 例如：通过早期的反馈来加速调优过程。

下图显示了 NNI 的体系结构。


.. raw:: html

   <p align="center">
   <img src="https://user-images.githubusercontent.com/16907603/92089316-94147200-ee00-11ea-9944-bf3c4544257f.png" alt="drawing" width="700"/>
   </p>


主要概念
------------


* 
  *Experiment（实验）*： 表示一次任务，例如，寻找模型的最佳超参组合，或最好的神经网络架构等。 它由 Trial 和自动机器学习算法所组成。

* 
  *搜索空间*：是模型调优的范围。 例如，超参的取值范围。

* 
  *Configuration（配置）*：配置是来自搜索空间的实例，每个超参都会有特定的值。

* 
  *Trial*：是一次独立的尝试，它会使用某组配置（例如，一组超参值，或者特定的神经网络架构）。 Trial 会基于提供的配置来运行。

* 
  *Tuner（调优器）*：一种自动机器学习算法，会为下一个 Trial 生成新的配置。 新的 Trial 会使用这组配置来运行。

* 
  *Assessor（评估器）*：分析 Trial 的中间结果（例如，定期评估数据集上的精度），来确定 Trial 是否应该被提前终止。

* 
  *训练平台*：是 Trial 的执行环境。 根据 Experiment 的配置，可以是本机，远程服务器组，或其它大规模训练平台（如，OpenPAI，Kubernetes）。

Basically, an experiment runs as follows: Tuner receives search space and generates configurations. These configurations will be submitted to training platforms, such as the local machine, remote machines, or training clusters. Their performances are reported back to Tuner. Then, new configurations are generated and submitted.

For each experiment, the user only needs to define a search space and update a few lines of code, and then leverage NNI built-in Tuner/Assessor and training platforms to search the best hyperparameters and/or neural architecture. There are basically 3 steps:

..

   Step 1: `Define search space <Tutorial/SearchSpaceSpec.rst>`__

   Step 2: `Update model codes <TrialExample/Trials.rst>`__

   Step 3: `Define Experiment <Tutorial/ExperimentConfig.rst>`__



.. raw:: html

   <p align="center">
   <img src="https://user-images.githubusercontent.com/23273522/51816627-5d13db80-2302-11e9-8f3e-627e260203d5.jpg" alt="drawing"/>
   </p>


For more details about how to run an experiment, please refer to `Get Started <Tutorial/QuickStart.rst>`__.

Core Features
-------------

NNI provides a key capacity to run multiple instances in parallel to find the best combinations of parameters. This feature can be used in various domains, like finding the best hyperparameters for a deep learning model or finding the best configuration for database and other complex systems with real data.

NNI also provides algorithm toolkits for machine learning and deep learning, especially neural architecture search (NAS) algorithms, model compression algorithms, and feature engineering algorithms.

Hyperparameter Tuning
^^^^^^^^^^^^^^^^^^^^^

This is a core and basic feature of NNI, we provide many popular `automatic tuning algorithms <Tuner/BuiltinTuner.md>`__ (i.e., tuner) and `early stop algorithms <Assessor/BuiltinAssessor.md>`__ (i.e., assessor). You can follow `Quick Start <Tutorial/QuickStart.rst>`__ to tune your model (or system). Basically, there are the above three steps and then starting an NNI experiment.

General NAS Framework
^^^^^^^^^^^^^^^^^^^^^

This NAS framework is for users to easily specify candidate neural architectures, for example, one can specify multiple candidate operations (e.g., separable conv, dilated conv) for a single layer, and specify possible skip connections. NNI will find the best candidate automatically. On the other hand, the NAS framework provides a simple interface for another type of user (e.g., NAS algorithm researchers) to implement new NAS algorithms. A detailed description of NAS and its usage can be found `here <NAS/Overview.rst>`__.

NNI has support for many one-shot NAS algorithms such as ENAS and DARTS through NNI trial SDK. To use these algorithms you do not have to start an NNI experiment. Instead, import an algorithm in your trial code and simply run your trial code. If you want to tune the hyperparameters in the algorithms or want to run multiple instances, you can choose a tuner and start an NNI experiment.

Other than one-shot NAS, NAS can also run in a classic mode where each candidate architecture runs as an independent trial job. In this mode, similar to hyperparameter tuning, users have to start an NNI experiment and choose a tuner for NAS.

Model Compression
^^^^^^^^^^^^^^^^^

NNI provides an easy-to-use model compression framework to compress deep neural networks, the compressed networks typically have much smaller model size and much faster
inference speed without losing performance significantlly. Model compression on NNI includes pruning algorithms and quantization algorithms. NNI provides many pruning and
quantization algorithms through NNI trial SDK. Users can directly use them in their trial code and run the trial code without starting an NNI experiment. Users can also use NNI model compression framework to customize their own pruning and quantization algorithms.

A detailed description of model compression and its usage can be found `here <Compression/Overview.rst>`__.

Automatic Feature Engineering
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Automatic feature engineering is for users to find the best features for their tasks. A detailed description of automatic feature engineering and its usage can be found `here <FeatureEngineering/Overview.rst>`__. It is supported through NNI trial SDK, which means you do not have to create an NNI experiment. Instead, simply import a built-in auto-feature-engineering algorithm in your trial code and directly run your trial code. 

The auto-feature-engineering algorithms usually have a bunch of hyperparameters themselves. If you want to automatically tune those hyperparameters, you can leverage hyperparameter tuning of NNI, that is, choose a tuning algorithm (i.e., tuner) and start an NNI experiment for it.

Learn More
----------


* `Get started <Tutorial/QuickStart.rst>`__
* `How to adapt your trial code on NNI? <TrialExample/Trials.rst>`__
* `What are tuners supported by NNI? <Tuner/BuiltinTuner.rst>`__
* `How to customize your own tuner? <Tuner/CustomizeTuner.rst>`__
* `What are assessors supported by NNI? <Assessor/BuiltinAssessor.rst>`__
* `How to customize your own assessor? <Assessor/CustomizeAssessor.rst>`__
* `How to run an experiment on local? <TrainingService/LocalMode.rst>`__
* `How to run an experiment on multiple machines? <TrainingService/RemoteMachineMode.rst>`__
* `How to run an experiment on OpenPAI? <TrainingService/PaiMode.rst>`__
* `Examples <TrialExample/MnistExamples.rst>`__
* `Neural Architecture Search on NNI <NAS/Overview.rst>`__
* `Model Compression on NNI <Compression/Overview.rst>`__
* `Automatic feature engineering on NNI <FeatureEngineering/Overview.rst>`__
