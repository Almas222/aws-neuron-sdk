.. _trn1-performance:

Trn1/Trn1n Performance
=======================

.. contents:: Table of contents
   :local:


*Last update:  September 15th, 2023*


.. _NLP:

Training Performance (Trn1 / Trn1n)
-----------------------------------

.. csv-table::
   :file: trn1_trn1n_nlp_data.csv
   :header-rows: 1

.. note::
         **TP (Tensor Parallel), PP (Pipeline Parallel) and DP (Data Parallel)** Topology configuration refers to the degrees of 3D Parallelism (How the model and data is sharded across neuron cores).

         TP and PP are specified in the run script and DP is calculated by dividing **world size**(Number of nodes/instances * Number of neuron cores per instance) by TP * PP degrees.

         For example : TP = 4, PP = 4 and Number of instances is 32 (trn1.32xlarge). The world size will be : 32 (num instances) * 32(neuron cores per instance) = 1024. Now, DP degree = 1024 (World size)/ 4 (TP) * 4 (PP) = 64

.. note::
         Read more about strong vs weak scaling here :ref:`neuron-training-faq`


Inference Performance
---------------------

.. tab-set::

   .. tab-item:: Throughput optimized

      .. df-table::
         :header-rows: 1

         df = pd.read_csv('throughput_data.csv')
         df_prices = pd.read_csv('trn1_instance_prices.csv')
         df = pd.merge(df,df_prices,on='Inst. Type')

         df['Cost per 1M inferences'] = ((1.0e6 / df['Throughput (inference/sec)']) * (df['On-Demand hourly rate'] / 3.6e3 )).map('${:,.3f}'.format)

         cols_to_show = ['Model','Scripts','Framework', 'Inst. Type', 'Task', 'Throughput (inference/sec)', 'Latency P50 (ms)', 'Latency P99 (ms)', 'Cost per 1M inferences', 'Application Type', 'Neuron Version', 'Run Mode', 'Batch Size','Sequence Length', 'Model Data Type','Compilation Autocast Data Type','OS Type']
         df = df[cols_to_show].sort_values(['Model', 'Cost per 1M inferences'])

         df['Throughput (inference/sec)'] = df['Throughput (inference/sec)'].round(2).astype('float',copy=True)
         int_cols = ['Latency P50 (ms)', 'Latency P99 (ms)']
         df[int_cols] = df[int_cols].round(2).astype('float',copy=True)

      .. note::
         **Cost per 1M inferences** is calculated using On-Demand hourly rate.

         **Real Time** application refers to batch size 1 inference for minimal latency. **Batch** application refers to maximum throughput with minimum cost-per-inference.

   .. tab-item:: Latency optimized

      .. df-table::
         :header-rows: 1

         df = pd.read_csv('latency_data.csv')
         df_prices = pd.read_csv('trn1_instance_prices.csv')
         df = pd.merge(df,df_prices,on='Inst. Type')

         df['Cost per 1M inferences'] = ((1.0e6 / df['Throughput (inference/sec)']) * (df['On-Demand hourly rate'] / 3.6e3 )).map('${:,.3f}'.format)

         cols_to_show = ['Model','Scripts','Framework', 'Inst. Type', 'Task', 'Throughput (inference/sec)', 'Latency P50 (ms)', 'Latency P99 (ms)', 'Cost per 1M inferences', 'Application Type', 'Neuron Version', 'Run Mode', 'Batch Size','Sequence Length', 'Model Data Type','Compilation Autocast Data Type','OS Type']
         df = df[cols_to_show].sort_values(['Model', 'Cost per 1M inferences'])

         df['Throughput (inference/sec)'] = df['Throughput (inference/sec)'].round(2).astype('float',copy=True)
         int_cols = ['Latency P50 (ms)', 'Latency P99 (ms)']
         df[int_cols] = df[int_cols].round(2).astype('float',copy=True)

      .. note::
         **Cost per 1M inferences** is calculated using On-Demand hourly rate.

         **Real Time** application refers to batch size 1 inference for minimal latency. **Batch** application refers to maximum throughput with minimum cost-per-inference.


Large Language Models Inference Performance
-------------------------------------------

.. tab-set::

   .. tab-item:: Throughput optimized

      .. df-table::
         :header-rows: 1

         df = pd.read_csv('trn1_throughput_data_LLM.csv')
         df_prices = pd.read_csv('trn1_instance_prices.csv')
         df = pd.merge(df,df_prices,on='Inst. Type')
         df['Cost per 1M inferences'] = ((1.0e6 / df['Throughput (tokens/second)']) * (df['On-Demand hourly rate'] / 3.6e3 )).map('${:,.3f}'.format)
         cols_to_show = ['Model','Scripts','Framework', 'Inst. Type', 'Task', 'Throughput (tokens/second)', 'Latency per Token P50 (ms)', 'Latency per Token P99 (ms)', 'Cost per 1M inferences', 'Application Type', 'Neuron Version', 'Run Mode', 'TP Degree',	'DP Degree', 'Batch Size', 'Sequence Length', 'Input Length', 'Output Length', 'Model Data Type','Compilation Autocast Data Type']
         df = df[cols_to_show].sort_values(['Model', 'Cost per 1M inferences'])
         df['Throughput (tokens/second)'] = df['Throughput (tokens/second)'].round(2).astype('float',copy=True)
         int_cols = ['Latency per Token P50 (ms)', 'Latency per Token P99 (ms)']
         df[int_cols] = df[int_cols].round(2).astype('float',copy=True)

      .. note::
         **Throughput (tokens/second)** counts both input and output tokens

         **Latency per Token** counts both input and output tokens

         **Cost per 1M inferences** is calculated using On-Demand hourly rate.

         **Real Time** application refers to batch size 1 inference for minimal latency. **Batch** application refers to maximum throughput with minimum cost-per-inference.
      

   .. tab-item:: Latency optimized

      .. df-table::
         :header-rows: 1

         df = pd.read_csv('trn1_latency_data_LLM.csv')
         df_prices = pd.read_csv('trn1_instance_prices.csv')
         df = pd.merge(df,df_prices,on='Inst. Type')
         df['Cost per 1M inferences'] = ((1.0e6 / df['Throughput (tokens/second)']) * (df['On-Demand hourly rate'] / 3.6e3 )).map('${:,.3f}'.format)
         cols_to_show = ['Model','Scripts','Framework', 'Inst. Type', 'Task', 'Throughput (tokens/second)', 'Latency per Token P50 (ms)', 'Latency per Token P99 (ms)', 'Cost per 1M inferences', 'Application Type', 'Neuron Version', 'Run Mode', 'TP Degree',	'DP Degree', 'Batch Size', 'Sequence Length', 'Input Length', 'Output Length', 'Model Data Type','Compilation Autocast Data Type']
         df = df[cols_to_show].sort_values(['Model', 'Cost per 1M inferences'])
         df['Throughput (tokens/second)'] = df['Throughput (tokens/second)'].round(2).astype('float',copy=True)
         int_cols = ['Latency per Token P50 (ms)', 'Latency per Token P99 (ms)']
         df[int_cols] = df[int_cols].round(2).astype('float',copy=True)
      
      .. note::
         **Throughput (tokens/second)** counts both input and output tokens

         **Latency per Token** counts both input and output tokens

         **Cost per 1M inferences** is calculated using On-Demand hourly rate.

         **Real Time** application refers to batch size 1 inference for minimal latency. **Batch** application refers to maximum throughput with minimum cost-per-inference.

