---
layout:     post
title:      Synapse in Brian2
subtitle:   Brian2中突触建模
date:       2019-10-29
author:     HSI
header-img: img/post-bg-desk.jpg
catalog: 	 true
tags:
    - Brian2
    - synapse
    - alpha function
    - COBA
    - CUBA
---
{% include lib/mathjax.html %}

How to implement synapses in Brian2?
===

**Summary**: This is the third post about the synapse in Brian2. After a half year, I have studied and used brian2 for longer time than one year ago.
I found some new methods and my past mistakes.
### Synapse type 
The title 'synapse type' is not the effect type -- Exi. or Inh. -- but the modeling method. In the perspective of modeling, synapse can
can be classified into current-based (CUBA) and conductance-based (COBA).

### Current-based synapse (CUBA)
It is easy to understand CUBA, which means that the modeled synapse directively represents the synaptic current in the neuron equation, shown as follows:
#### exponential current
```py
tau = 5*ms
syn_eqs = Equations('dI_syn/dt = -I_syn/tau : amp')
eqs = (Equations('dvm/dt = (gl*(El - vm) + I_syn)/C : volt') +
       syn_eqs)
group = NeuronGroup(N, eqs, threshold='vm>-50*mV', reset='vm=-70*mV')
syn = Synapses(source, group, on_pre='I_syn += 1*nA')
# ... connect synapses, etc.
```
The equation is a exponential current with **only one PDE**.
#### alpha current
```py
tau = 2.5*ms
syn_eqs = Equations('''dI_syn/dt = (s - I_syn)/tau : amp
                       ds/dt = -s/tau : amp''')
group = NeuronGroup(N, eqs, threshold='vm>-50*mV', reset='vm=-70*mV')
syn = Synapses(source, group, on_pre='s += 1*nA')
# ... connect synapses, etc.
```
The alpha current needs **two PDEs** to get the alpha-shape function.

Similarly, we now can easily understand the conductance-based synapse.
### Conductance-based synapse (COBA)
The conductance-based synapse represent the conductance of synapses multiplying the corresponding potential difference to get the synaptic currents.
Comparing to the current-based current, (I think) COBA fits the biological synapses better.
### exponential conductance
```py
tau = 5*ms; E = 0*mV
syn_eqs = Equations('dg_syn/dt = -g_syn/tau : siemens')
eqs = (Equations('dvm/dt = (gl*(El - vm) + g_syn*(E - vm))/C : volt') +
       syn_eqs)
group = NeuronGroup(N, eqs, threshold='vm>-50*mV', reset='vm=-70*mV')
syn = Synapses(source, group, on_pre='g_syn += 10*nS')
# ... connect synapses, etc.
```
,and
### alpha conductance
```py
tau = 2.5*ms; E = 0*mV
syn_eqs = Equations('''dg_syn/dt = (s - g_syn)/tau : siemens
                       ds/dt = -s/tau : siemens''')
group = NeuronGroup(N, eqs, threshold='vm>-50*mV', reset='vm=-70*mV')
syn = Synapses(source, group, on_pre='s += 10*nS')
# ... connect synapses, etc.

```
The alpha conductance-based synapse is used in the paper I have mentioned in the post [How to implement the alpha-function-type synapse?](https://haowillsi.github.io/2019/01/21/Further-Study-on-Synapse/).

**In a summary**, this four synapse are the very modeling methods used in theoretical neuroscience works for most times. 
There are more complex models, eg. NMDA, AMPA, etc. whose progaraming codes are developed based on the models I mentioned above. 
