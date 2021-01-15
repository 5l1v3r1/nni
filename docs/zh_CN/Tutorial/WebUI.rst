Web 界面
=====

Experiments managerment
-----------------------

Click the tab ``All experiments`` on the nav bar.

.. image:: ../../img/webui-img/managerExperimentList/experimentListNav.png
   :target: ../../img/webui-img/managerExperimentList/experimentListNav.png
   :alt: ExperimentList nav



* On the ``All experiments`` page, you can see all the experiments on your machine. 

.. image:: ../../img/webui-img/managerExperimentList/expList.png
   :target: ../../img/webui-img/managerExperimentList/expList.png
   :alt: Experiments list



* When you want to see more details about an experiment you could click the trial id, look that:

.. image:: ../../img/webui-img/managerExperimentList/toAnotherExp.png
   :target: ../../img/webui-img/managerExperimentList/toAnotherExp.png
   :alt: See this experiment detail



* If has many experiments on the table, you can use the ``filter`` button.

.. image:: ../../img/webui-img/managerExperimentList/expFilter.png
   :target: ../../img/webui-img/managerExperimentList/expFilter.png
   :alt: filter button



查看概要页面
-----------------

Click the tab ``Overview``.


* On the overview tab, you can see the experiment information and status and the performance of ``top trials``.


.. image:: ../../img/webui-img/full-oview.png
   :target: ../../img/webui-img/full-oview.png
   :alt: overview



* If you want to see experiment search space and config, please click the right button ``Search space`` and ``Config`` (when you hover on this button).

   1. Search space file:


      .. image:: ../../img/webui-img/searchSpace.png
         :target: ../../img/webui-img/searchSpace.png
         :alt: searchSpace



   2. Config file:


      .. image:: ../../img/webui-img/config.png
         :target: ../../img/webui-img/config.png
         :alt: config



* You can view and download ``nni-manager/dispatcher log files`` on here.


.. image:: ../../img/webui-img/review-log.png
   :target: ../../img/webui-img/review-log.png
   :alt: logfile



* 如果 Experiment 包含了较多 Trial，可改变刷新间隔。


.. image:: ../../img/webui-img/refresh-interval.png
   :target: ../../img/webui-img/refresh-interval.png
   :alt: refresh




* You can review and download the experiment results(``experiment config``, ``trial message`` and ``intermeidate metrics``) when you click the button ``Experiment summary``.


.. image:: ../../img/webui-img/summary.png
   :target: ../../img/webui-img/summary.png
   :alt: summary



* You can change some experiment configurations such as ``maxExecDuration``, ``maxTrialNum`` and ``trial concurrency`` on here.


.. image:: ../../img/webui-img/edit-experiment-param.png
   :target: ../../img/webui-img/edit-experiment-param.png
   :alt: editExperimentParams



* You can click the icon to see specific error message and ``nni-manager/dispatcher log files`` by clicking ``Learn about`` link.


.. image:: ../../img/webui-img/experimentError.png
   :target: ../../img/webui-img/experimentError.png
   :alt: experimentError




* You can click ``About`` to see the version and report any questions.

查看任务默认指标
-----------------------


* Click the tab ``Default Metric`` to see the point graph of all trials. 悬停鼠标来查看默认指标和搜索空间信息。


.. image:: ../../img/webui-img/default-metric.png
   :target: ../../img/webui-img/default-metric.png
   :alt: defaultMetricGraph



* Click the switch named ``optimization curve`` to see the experiment's optimization curve.


.. image:: ../../img/webui-img/best-curve.png
   :target: ../../img/webui-img/best-curve.png
   :alt: bestCurveGraph


查看超参
--------------------

Click the tab ``Hyper Parameter`` to see the parallel graph.


* You can ``add/remove`` axes and drag to swap axes on the chart.
* 可选择百分比查看最好的 Trial。


.. image:: ../../img/webui-img/hyperPara.png
   :target: ../../img/webui-img/hyperPara.png
   :alt: hyperParameterGraph



查看 Trial 运行时间
-------------------

Click the tab ``Trial Duration`` to see the bar graph.


.. image:: ../../img/webui-img/trial_duration.png
   :target: ../../img/webui-img/trial_duration.png
   :alt: trialDurationGraph



查看 Trial 中间结果
------------------------------------

Click the tab ``Intermediate Result`` to see the line graph.


.. image:: ../../img/webui-img/trials_intermeidate.png
   :target: ../../img/webui-img/trials_intermeidate.png
   :alt: trialIntermediateGraph



Trial 可能在训练过程中有大量中间结果。 为了更清楚的理解一些 Trial 的趋势，可以为中间结果图设置过滤。

这样可以发现 Trial 在某个中间结果上会变得更好或更差。 这表明它是一个重要的并相关的中间结果。 如果要仔细查看这个点，可以在 #Intermediate 中输入其 X 坐标。 并输入这个中间结果的指标范围。 在下图中，选择了 No。 并将指标范围设置为了 0.8 -1。


.. image:: ../../img/webui-img/filter-intermediate.png
   :target: ../../img/webui-img/filter-intermediate.png
   :alt: filterIntermediateGraph



查看 Trial 状态
------------------

Click the tab ``Trials Detail`` to see the status of all trials. 特别是：


* Trial 详情：Trial 的 id，持续时间，开始时间，结束时间，状态，精度和搜索空间文件。


.. image:: ../../img/webui-img/detail-local.png
   :target: ../../img/webui-img/detail-local.png
   :alt: detailLocalImage



* The button named ``Add column`` can select which column to show on the table. 如果 Experiment 的最终结果是 dict，则可以在表格中查看其它键。 You can choose the column ``Intermediate count`` to watch the trial's progress.


.. image:: ../../img/webui-img/addColumn.png
   :target: ../../img/webui-img/addColumn.png
   :alt: addColumnGraph



* If you want to compare some trials, you can select them and then click ``Compare`` to see the results.


.. image:: ../../img/webui-img/select-trial.png
   :target: ../../img/webui-img/select-trial.png
   :alt: selectTrialGraph


.. image:: ../../img/webui-img/compare.png
   :target: ../../img/webui-img/compare.png
   :alt: compareTrialsGraph



* 支持通过 id，状态，Trial 编号， 以及参数来搜索。


.. image:: ../../img/webui-img/search-trial.png
   :target: ../../img/webui-img/search-trial.png
   :alt: searchTrial



* You can use the button named ``Copy as python`` to copy the trial's parameters.


.. image:: ../../img/webui-img/copyParameter.png
   :target: ../../img/webui-img/copyParameter.png
   :alt: copyTrialParameters



* 如果在 OpenPAI 或 Kubeflow 平台上运行，还可以看到 hdfsLog。


.. image:: ../../img/webui-img/detail-pai.png
   :target: ../../img/webui-img/detail-pai.png
   :alt: detailPai



* 中间结果图：可在此图中通过点击 intermediate 按钮来查看默认指标。


.. image:: ../../img/webui-img/intermediate.png
   :target: ../../img/webui-img/intermediate.png
   :alt: intermeidateGraph



* Kill: 可终止正在运行的任务。


.. image:: ../../img/webui-img/kill-running.png
   :target: ../../img/webui-img/kill-running.png
   :alt: killTrial

