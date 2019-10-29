---
layout:     post
title:      Further Study on Synapse
subtitle:   Brian2中突触的进一步学习
date:       2019-01-21
author:     HSI
header-img: img/post-bg-desk.jpg
catalog: 	 true
tags:
    - Brian2
    - synapse
    - alpha function
---
{% include lib/mathjax.html %}

How to implement the alpha-function-type synapse?
===

**Summary**: In the last post (very long long ago), I studied the Synapse. However, in fact I still didn't know how to implement the synapse 
model from the equation (eg. alpha function) in the paper I read. Therefore, I turned to the Brian Google Groups and my senior who ever used 
the Brian. 
### Learning 
In the paper I have read, most synapses were modeled as alpha function. As is shown in the pic below:
![](https://github.com/HardworkingChris/Brian2_Learning/raw/master/3-synapse/synapse_alpha.PNG)  
, which is from the paper [_Propagation of Firing Rate in a Feed-Forward Neuronal Network_](https://journals.aps.org/prl/abstract/10.1103/PhysRevLett.96.018103)

I learned that in Brian2 the authors give an introduction named [_Converting from integrated form to ODEs_](https://brian2.readthedocs.io/en/stable/user/converting_from_integrated_form.html)
to show how to implement ordinary differential equations.
Some examples shown in the introduction include the Alpha synapse, $$V(t)=(t/τ)e^{−t/τ}$$:
```py
eqs = '''
dV/dt = (x-V)/tau : 1
dx/dt = -x/tau    : 1
'''
on_pre = 'x += w'
```
### How  
According to the introduction about ODEs implementation and examples, I write the python-brian2 code to implement excitatory and inhibitory 
synaptic currents.

#### excitatory synaptic current
(1) synaptic conductance (alpha function) equation <br>
```py
eqs_synapse ='''
dgsyn/dt = (ge-gsyn)*(1./taue) : siemens
dge/dt = -ge*(1./taue) : siemens
'''
```
(2) synapse equation in Neuron eq. <br>
```py
eqs_neuron ='''
dv/dt = (- g_na*(m*m*m)*h*(v-ENa) - g_k*(n*n*n*n)*(v-EK) - gl*(v-El) + gsyn*(Ee-v) + I )/Cm: volt
'''+eqs_synapse
```
(3) synaptic weight <br>
```py
con_ee.append(Synapses(group[m*groupsize_e:(m+1)*groupsize], group[m*groupsize:(m+1)*groupsize], 'w: 1', on_pre='ge += w'))#updating
con_ee.connect(p=epsilon)#synaptic connacting probability
con_ee.w = J_e#synaptic weight
```
#### inhibitory synaptic current
The inhibitory synaptic current is similar as the excitatory one.
