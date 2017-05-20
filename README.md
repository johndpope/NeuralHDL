# NeuralHDL

NeuralHDL is an internal DSL in scala for hardware design geared towards neural networks. The goal of this project will be to create (or expand) existing tool sets like [DeepLearningForJ](https://deeplearning4j.org/) or [CAFFE](http://caffe.berkeleyvision.org/) to directly generate and interface to hardware designs. This project is based on an existing general purpose hardware DSL ([ScalaDL](https://github.com/andywag/ScalaDL)) which was used in the design of a highly parallel optical modem. 

Given the nature of neural networks there is no reason that a CAFFE like description shown below can't be directly mapped to a hardware structure which is far easier. This design tool will use an internal DSL rather than JSON but the concepts are the same with a simple description describing the hardware. 

```json
layer {
  name: "conv1"
  type: "Convolution"
  param { lr_mult: 1 }
  param { lr_mult: 2 }
  convolution_param {
    num_output: 20
   }
  bottom: "data"
  top: "conv1"
}
```

This project and design methodology is the culmination of decades of work struggling to efficiently design signal processing hardware. Signal processing hardware in general contains repeatable structured hardware which is easy to visualize and describe mathematically yet difficult to describe in HDLs or HLS tools. As an architect, I wanted to conveniently specify the hardware in an abstract fashion but still control the resulting design which is not possible with other tools. Some background of this is described [here](#background). 

## Getting Started

This project is a scala project using SBT as a build tool and Intellij for code development and verilator for RTL simulation. 

1. http://www.scala-sbt.org/
2. https://www.jetbrains.com/idea/
3. https://www.veripool.org/wiki/verilator

To get started, download this project and import into Intellij or run using the sbt tool. Verilator is a free simulator which is used for testing the generated RTL and is not required. 

## Examples

The examples shown below are not full level examples of neural networks but show the method and structure which is used for creating and testing the building blocks. Higher level functionallity will be added as the building block development progresses. 



### Sigmoid Function
[Sigmoid.scala](Sigmnoid.scala)

### Neuron

### Neural Stage

## Background
I have been attempting to automate signal processing hardware design for longer than I would like to admit. Through these years there have been many commercial tools to solve this problem but have all suffered from the same inherent flaw in having the tools automate architeture.  

1. Tools are not good at architecture

As a signal processing/hardware architect, I want a convenient way to describe the design without giving up control of how the architecture is mapped. Most tools including the current C++ HLS languages do not support an entry mechanism which gives architects the power/control to specify their design. This approach works but tends to lead to inefficient and difficult to support designs. I have yet to see a design which did not have issues in development and greater issues when the original designers left.  

The alternate approach used here is an internal DSL which allows "architect/designer" to define his own abstraction to be able to completely specify his design in an abstract way. Other tools have been built following this approach (including a prior version of this tool) which have not had great adoption. 

1. https://github.com/VeriScala/VeriScala
2. https://chisel.eecs.berkeley.edu/

The issue with these languages is that hardware designers in general have extremely limited software abilities and the initial design of the DSL can be challenging. The goal of this project is not to solve the general problem, but instead use it to create a tool set to enable simple designs to be specified and implemented by non-hardware savvy people. In general this approach would not be successful but should work well for a targetted solution. The machine learning engineer can work with a familiar language while at the same time the hardware designer can work with a familiar language. 

## General HDL language issues

HDL languages have advantages and disadvantages. For structured designs like signal processing the disadvantages far outweigh the advantages so I have spent years attempting to automate the design of structured operations. 

1. HDLs (Verilog-VHDL) are good at timing, low-level bit manipulation and control
2. HDLs are not good at parameterization and abstraction or automating structured designs

These two conflicting issues make automation problematic since it is beneficial to write control logic using an HDL but abstract more structured items using a higher level language. This led to various incarnations of hybrid design tools which only used partial generation leading to issues. Internal DSL solve this issues by allowing control over the syntax so the complete design can be included in the same language and structure


