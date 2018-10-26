Gennerate simple data to evaluate algorithm 

## TO DO :
### Gen data again:
NOTE: Using test_func without decode_batch (ctc) is better than when using ctc for predicting phase
+ clean set (expected rnn model > 97%)
+ noise set (expected < acc of clean set)
+ padding set
=> so then we can apply IQA to imporve acc on noise set and padding set
```
1. Change function predict in Recognizor with test function (only X as input not length)
2. pad background identity card to same reality situation.
3. Create thin datase for 1080 train
4. **survey noise and padding method **
```

## DONE
```
1. add noise (same real) for back  [DONE]
2. Imporve training rnn
```

## PROBLEM IN TRAINING :
1. loss visualization not smooth
### REASON MAYBE:
1. Augument data too much => variation
2. batch size small
3. too much noise
### SOLVE:
1. [x] Increase batch size
2. About data generation

    2.0 [x] Train with shuffe gen data MORE time before gen new data.
    2.1 [x] Reduce random in gen function data: rand in font, rand in noise 
    2.2 [x] Use less noise: only use noise in real intuation.
3. [] Standardize data
4. [] check preprocessing of train
5. [] Too much regularization : dropout, batchnorm, weight L2. Reduce them.
	 (Search more ...)
6. [x] Switch mode Train to Test in layers: Batch Norm, Dropout, ... Don't use  Dropout in test mode For example.
7. [x] Decrease learning rate. (use sgd (0.003) instead of using Adadelta)
8. [] Init weight properly (maybe).
9. [x] Use dropout in LSTM : LSTM(dropout=0.4). Dropout is diff dropout recurrent.
10. [x] Save the best model in entire training process=> compare loss
### Explain Solution
1. [x] small batch size => more diff between batches => loss in between batches diff
2. [x] Train in small duration cause cannot learning deep in training set
3. []
4. []
5. []
6. [x] With dropout cannot app dropout in testing because when test we need to use all weight.
```
    Note that if your model has a different behavior in training and testing phase (e.g. if it uses Dropout, BatchNormalization, etc.), you will need to pass the learning phase flag to your function:

    get_3rd_layer_output = K.function([model.layers[0].input, K.learning_phase()], [model.layers[3].output])
    output in test mode = 0
    layer_output = get_3rd_layer_output([x, 0])[0]
```
7. [x] Large lr => hard to converge.
8. []
9. [x] Dropout for input and dropout for recurrent state. BatchNorm : Conv2d(Non-activation)->BatchNorm->Activation(ReLU). https://stackoverflow.com/questions/39691902/ordering-of-batch-normalization-and-dropout-in-tensorflow 
```
    Should use batch before Activation :(
    https://github.com/ducha-aiki/caffenet-benchmark/blob/master/batchnorm.md
```
## PROBLEM IN TESTING
1. [] Low accuracy : add more test sample to training sample.

    1.1 [x] add red sin background for training 
    1.2 [] add padded id area when id card is padded.
    1.3 [x] find font for id card.
## Noise maybe in real on ID card:
Using example on imgaug
1. [x] Add(-20,20)
2. [x] AdditiveGaussionNoise
3. [x] Salt p=(0->0.03)
4. [x] Pepper p= (0->0.03)
5. [x] GaussianBlur sigma=(0->0.25 or 0.5)
6. [x] AverageBlur k=(1, 3) 
7. [x] MedianBlur k=(1,3)
