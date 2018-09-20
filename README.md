# ETTTS_pytorch
pytorch implementation of https://arxiv.org/abs/1710.08969


### TODO
#### High level
 - [ ] get any NLP network working
 - [ ] get any audio network working
 - [ ] try
     - [ ] chainer - https://docs.chainer.org/en/stable/
     - [ ] gluon - https://medium.com/apache-mxnet/mxnet-gluon-in-60-minutes-3d49eccaf266
     - [x] pytorch
     

#### ETTTS - convolutional TTS 
- [ ] https://arxiv.org/abs/1710.08969
    - [x] read
    - [x] understand math
    - [x] draw architecture
    - [ ] implement in pytorch
        - [x] get data
        - [x] preprocess data
        - [x] char embed
        - [x] 1d conv
            - [x] fix causality
        - [x] 1d transpose conv oooooh
        - [x] highway connection/highway convolution
        - [x] weights initialize
        - [x] textenc
        - [x] audioenc
        - [x] attention
            - [x] guided
            - [ ] forcibly incremental
        - [x] audiodec
        - [x] ssrn
        - [x] impl loss functions
        - [x] train text2Mel
        - [x] train SSRN
        - [x] get GPU training working
            - [x] collab
            - [x] google cloud
            - [x] make backwards compatible w/ CPU
        - [x] bigger batch size - gpu mem usge at < 10%
            - [x] might have to increase cores for dataloader - 5 cores about saturates gpu  at batch size 16
            - [x] pretty sure model limited by fetcher speed
        - [x] checkpoint models % training
            - [x] remember to call model.eval() on load chkpt to make sure layers are in evaluation (as opposed to training mode)
            - [x] combine checkpointing logic for text2mel and ssrn by combining the text2Mel,audioDec,attention models into one class
            - save model results also
                - [ ] plots of attention,mel,fft
                - [ ] generated sound
                - [ ] model speed it/sec on cpu and gpu
        - [x] different checkpoint paths for different model params
            - [ ] work smthg out that prevents loading models w/ conflicting hyperparams
            - [ ] automatic cold start i.e. don't have to specify load = 1|0
        - [x] implement model params
            - [ ] nonsep vs sep vs super sep
            - [ ] batch vs layer vs weight vs instance vs group norm
            - [x] alpha
            - [x] learning rate
            - [x] chunk size (1 default for paper)
        - [ ] abstract class/fun for training/checkpointing/loss monitoring
        - [ ] create train dispatcher to train different hyperparameter combinations on different gpus
            - [x] request gpu limit increase -> 4
            - [ ] hyperparam queue?
        - [ ] test out if concatenating mel and text enc makes sense
        - [x] combine the text2Mel,audioDec,attention models into one class
        - [x] generate text2Mel
        - [x] generate SSRN
        - [x] fix inference memory leak
            - with ch.no_grad()
        - [ ] train text2Mel and SSRN together
        - [ ] chunked generation - train network to encode multiple timesteps at a time
        - [x] hyperparams class
            - [ ] add initialization?
        - [x] separate training code from model code
        - [x] separate eval code from training code
        - [ ] set behavior at preempt to restart and resume training 
        - [ ] split train test
        - [ ] separability
            - [x] non sep
            - [ ] sep
            - [ ] super sep
            - [ ] get rid of unnecesary separability params for separable convolutions
        - [ ] normalization
            - [x] batch norm
            - [ ] layer norm
            - [ ] instance norm
            - [ ] group norm
            - [ ] get idea for learning rate
        - [ ] decay
        - [ ] gradient clipping
        - [ ] pad from other direction? - seemed like attention model trained from end of input to beginning
        - [x] get some NULL character going for padding - alternatively modify c2i to not map any character to 0
- [x] use as reference
    - [x] https://github.com/Kyubyong/dc_tts
    - [x] https://github.com/eazhary/dctts2
    - [x] https://github.com/joisino/chainer-ETTTS
    - [x] find difference
- [ ] citations
        - [ ] main insipration: https://arxiv.org/abs/1705.03122
- [ ] cited by



#### Further work
- [ ] waveRNN
    - [ ] https://arxiv.org/pdf/1802.08435.pdf
    - [ ] /Users/aduriseti/Documents/2018spring/tesla/WaveRNN-master
    - [ ] /Users/aduriseti/Documents/2018spring/tesla/TensorFlow-Efficient-Neural-Audio-Synthesis-master
- [ ] waveNet
    - [ ] https://arxiv.org/abs/1609.03499
- [ ] streaming spectrogram generation
    - [ ] https://pdfs.semanticscholar.org/095a/ce7fbffb4b55ba6e71f6c06566fa4de67d69.pdf
- [ ] gan TTS/voice conversion (VC)
    - [ ] https://github.com/r9y9/gantts
- [ ] styleNN - if only for the dataset
    - [ ] http://imanmalik.com/cs/2017/06/05/neural-style.html
- [ ] deepVoice3
    - [ ] https://arxiv.org/abs/1710.07654
- [ ] general optimization
    - [ ] squeezenet
        - [ ] https://arxiv.org/abs/1602.07360
    - [ ] mobilenet
        - [x] https://arxiv.org/abs/1704.04861
        - depthwise separable /w memory managemnt opt and op vectorizing opt
        - [x] understand depthwise sep & complexity
        - [x] understand memory management opt
        - [x] understand op opt
        - [ ] look at related papers
        - [x] try out channel thinning parameter $\alpha$
    - [x] xception:
        - [x] https://arxiv.org/abs/1610.02357
        - pure depthwise separable w/ residual connections - demonstrated state of the art performance on ImageNet and faster training
    - [ ] depthwise sep convolution for NMT
        - [x] https://openreview.net/forum?id=S1jBcueAb
        - super separability: group ptwise ops also
        - they found parameter savings from serparation/super separation to be superior to param savings from dilation
        - [x] try it out 
    - [ ] sparsity constraints w/ pruning
        - [ ] for RNN i.e. waveRNN
        - [ ] forn CNN - saw package online - https://github.com/jacobgil/pytorch-pruning
            - Note: supposedly not equally efficient to train
    - [ ] network weight decomposition
        - [ ] https://jacobgil.github.io/deeplearning/tensor-decompositions-deep-learning