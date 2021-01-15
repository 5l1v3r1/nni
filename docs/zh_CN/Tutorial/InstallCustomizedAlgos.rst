
如何将自定义的算法安装为内置的 Tuner，Assessor 和 Advisor
=======================================================================================

.. contents::

概述
--------

NNI 提供了大量可用于超参优化的 `内置 Tuner <../Tuner/BuiltinTuner.rst>`_, `Advisor <../Tuner/HyperbandAdvisor.rst>`__ 和 `Assessor <../Assessor/BuiltinAssessor.rst>`__ ，其他算法可在 NNI 安装后，通过 ``nnictl algo register --meta <path_to_meta_file>`` 安装。 可通过 ``nnictl package list`` 命令查看其它算法。

NNI 中，还可以创建自定义的 Tuner，Advisor 和 Assessor。 并根据 Experiment 配置文件的说明来使用这些自定义的算法，可参考 `自定义 Tuner <../Tuner/CustomizeTuner.rst>`_ ， `Advisor <../Tuner/CustomizeAdvisor.rst>`__ 和 `Assessor <../Assessor/CustomizeAssessor.rst>`__。

用户可将自定义的算法作为内置算法安装，以便像其它内置 Tuner、Advisor、Assessor 一样使用。 更重要的是，这样更容易向其他人分享或发布自己实现的算法。 自定义的 Tuner、Advisor、Assessor 可作为内置算法安装到 NNI 中，安装完成后，可在 Experiment 配置文件中像内置算法一样使用。 例如，将自定义的算法 ``mytuner`` 安装到 NNI 后，可在配置文件中直接使用：

.. code-block:: yaml

   tuner:
     builtinTunerName: mytuner

将自定义的算法安装为内置的 Tuner，Assessor 或 Advisor
------------------------------------------------------------------------

可参考下列步骤来构建自定义的 Tuner、Assessor、Advisor，并作为内置算法安装。

1. 创建自定义的 Tuner、Assessor、Advisor
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

参考下列说明来创建：


* `自定义 Tuner <../Tuner/CustomizeTuner.rst>`_
* `自定义 Assessor <../Assessor/CustomizeAssessor.rst>`_
* `自定义 Advisor <../Tuner/CustomizeAdvisor.rst>`_

2. (可选) 创建 Validator 来验证 classArgs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

NNI 提供了 ``ClassArgsValidator`` 接口，自定义的算法可用它来验证 Experiment 配置文件中传给构造函数的 classArgs 参数。
``ClassArgsValidator`` 接口如下：

.. code-block:: python

   class ClassArgsValidator(object):
       def validate_class_args(self, **kwargs):
           """
           Experiment 配置中的 classArgs 字段会作为 dict
           传入到 kwargs。
           """
           pass

例如，可将 Validator 如下实现：

.. code-block:: python

   from schema import Schema, Optional
   from nni import ClassArgsValidator

   class MedianstopClassArgsValidator(ClassArgsValidator):
       def validate_class_args(self, **kwargs):
           Schema({
               Optional('optimize_mode'): self.choices('optimize_mode', 'maximize', 'minimize'),
               Optional('start_step'): self.range('start_step', int, 0, 9999),
           }).validate(kwargs)

在 Experiment 启动时，会调用 Validator，检查 classArgs 字段是否正确。

3. 将自定义算法包安装到 python 环境 中
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

首先，自定义的算法需要被打成 python 包。 然后你可以通过以下命令把算法包安装到 python 环境中：


* 在包目录中运行 ``python setup.py develop``，此命令会在开发者模式下安装包。如果算法正在开发中，推荐使用此命令。
* 在包目录中运行 ``python setup.py bdist_wheel`` 命令，会构建 whl 文件。 可通过 ``pip3 install sklearn`` 命令来安装。

4. 准备安装源
^^^^^^^^^^^^^^^^^^^^

使用以下关键词创建 YAML 文件：


* ``algoType``: 算法类型，可为 ``tuner``, ``assessor``, ``advisor``
* ``builtinName``: 在 Experiment 配置文件中使用的内置名称
* `className`: Tuner 类名，包括模块名，例如：``demo_tuner.DemoTuner``
* `classArgsValidator`: 类的参数验证类 validator 的类名，包括模块名，如：``demo_tuner.MyClassArgsValidator``

YAML 文件示例：

.. code-block:: yaml

   algoType: tuner
   builtinName: demotuner
   className: demo_tuner.DemoTuner
   classArgsValidator: demo_tuner.MyClassArgsValidator

5. 将自定义算法包安装到 NNI 中
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

运行以下命令将自定义算法加入到 NNI 的内置算法中：

.. code-block:: bash

   nnictl algo register --meta <path_to_meta_file>

``<path_to_meta_file>`` 是上一节创建的 YAML 文件的路径。

Reference `customized tuner example <#example-register-a-customized-tuner-as-a-builtin-tuner>`_ for a full example.

在 Experiment 中使用安装的算法
--------------------------------------------------

在自定义算法安装后，可用其它内置 Tuner、Assessor、Advisor 的方法在 Experiment 配置文件中使用，例如：

.. code-block:: yaml

   tuner:
     builtinTunerName: demotuner
     classArgs:
       # 可选项: maximize, minimize
       optimize_mode: maximize

使用 ``nnictl algo`` 管理内置的算法
-----------------------------------------------

列出已安装的包
^^^^^^^^^^^^^^^^^^^^^^^

运行以下命令列出已安装的包：

.. code-block:: bash

   nnictl algo list
   +-----------------+------------+-----------+--------=-------------+------------------------------------------+
   |      Name       |    Type    | Source    |      Class Name      |               Module Name                |
   +-----------------+------------+-----------+----------------------+------------------------------------------+
   | TPE             | tuners     | nni       | HyperoptTuner        | nni.hyperopt_tuner.hyperopt_tuner        |
   | Random          | tuners     | nni       | HyperoptTuner        | nni.hyperopt_tuner.hyperopt_tuner        |
   | Anneal          | tuners     | nni       | HyperoptTuner        | nni.hyperopt_tuner.hyperopt_tuner        |
   | Evolution       | tuners     | nni       | EvolutionTuner       | nni.evolution_tuner.evolution_tuner      |
   | BatchTuner      | tuners     | nni       | BatchTuner           | nni.batch_tuner.batch_tuner              |
   | GridSearch      | tuners     | nni       | GridSearchTuner      | nni.gridsearch_tuner.gridsearch_tuner    |
   | NetworkMorphism | tuners     | nni       | NetworkMorphismTuner | nni.networkmorphism_tuner.networkmo...   |
   | MetisTuner      | tuners     | nni       | MetisTuner           | nni.metis_tuner.metis_tuner              |
   | GPTuner         | tuners     | nni       | GPTuner              | nni.gp_tuner.gp_tuner                    |
   | PBTTuner        | tuners     | nni       | PBTTuner             | nni.pbt_tuner.pbt_tuner                  |
   | SMAC            | tuners     | nni       | SMACTuner            | nni.smac_tuner.smac_tuner                |
   | PPOTuner        | tuners     | nni       | PPOTuner             | nni.ppo_tuner.ppo_tuner                  |
   | Medianstop      | assessors  | nni       | MedianstopAssessor   | nni.medianstop_assessor.medianstop_...   |
   | Curvefitting    | assessors  | nni       | CurvefittingAssessor | nni.curvefitting_assessor.curvefitt...   |
   | Hyperband       | advisors   | nni       | Hyperband            | nni.hyperband_advisor.hyperband_adv...   |
   | BOHB            | advisors   | nni       | BOHB                 | nni.bohb_advisor.bohb_advisor            |
   +-----------------+------------+-----------+----------------------+------------------------------------------+

卸载内置算法
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

运行以下命令卸载已安装的包：

``nnictl algo unregister <包名称>``

例如：

``nnictl algo unregister demotuner``


Porting customized algorithms from v1.x to v2.x
-----------------------------------------------

All that needs to be modified is to delete ``NNI Package :: tuner`` metadata in ``setup.py`` and add a meta file mentioned in `4. Prepare meta file`_. Then you can follow `Register customized algorithms as builtin tuners, assessors and advisors`_ to register your customized algorithms.

Example: Register a customized tuner as a builtin tuner
-------------------------------------------------------

You can following below steps to register a customized tuner in ``nni/examples/tuners/customized_tuner`` as a builtin tuner.

Install the customized tuner package into python environment
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

There are 2 options to install the package into python environment:

Option 1: install from directory
""""""""""""""""""""""""""""""""

From ``nni/examples/tuners/customized_tuner`` directory, run:

``python setup.py develop``

This command will build the ``nni/examples/tuners/customized_tuner`` directory as a pip installation source.

Option 2: install from whl file
"""""""""""""""""""""""""""""""

Step 1: From ``nni/examples/tuners/customized_tuner`` directory, run:

``python setup.py bdist_wheel``

This command build a whl file which is a pip installation source.

Step 2: Run command:

``pip install dist/demo_tuner-0.1-py3-none-any.whl``

Register the customized tuner as builtin tuner:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Run following command:

``nnictl algo register --meta meta_file.yml``

Check the registered builtin algorithms
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Then run command ``nnictl algo list``\ , you should be able to see that demotuner is installed:

.. code-block:: bash

   +-----------------+------------+-----------+--------=-------------+------------------------------------------+
   |      Name       |    Type    |   source  |      Class Name      |               Module Name                |
   +-----------------+------------+-----------+----------------------+------------------------------------------+
   | demotuner       | tuners     |    User   | DemoTuner            | demo_tuner                               |
   +-----------------+------------+-----------+----------------------+------------------------------------------+
