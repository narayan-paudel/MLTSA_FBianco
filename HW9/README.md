## scope
This homework is quite open ended. There is no scheleton notebook for it. 

Use the PLAsTiCC challenge data you used in HW6 and 7 and extract features from the time series via autoencoder. 

Then augment the feature space you used in HW7 with the new features created by the autoencoder, and see if this improves the classification. 

## Challenges

For each object in the dataset there are 7 time series for 7 optical filters. Each time series is composed of a series of time stamps and a series of fluxes (i.e. the time series are sparsely sampled).

1. Choose if you want to treat each filter for each object separately, or together. From a domain point of view, the relationship between what happens in different filters (i.e. at different wavelengths) is important. From a practical point of view ignoring it may be easier. Your choice

2. You **must choose a common time frame for all observations**: for example for each *object* choose the timestamps of the first observation and set it to 0. Cound time forward in days from that point. 

3. Choose if you want to interpolate the time series (e.g. cubic interpolation, spline, gaussian processes) and extract evenly sampled values *or* you want to deal with the sparsely sampled time series. 
 - in the first case you will create a function f(t) that allows you to extract the flux at N points in time. Now your time series are all evenly sampled and of course all the same size. This is the best option, i.e. the option that gives you the best chance of getting a good result, but you will have to work out the interpolation which may not be easy. It is not mandatory.
 - in the second case you need to preserve the time stamps and reprganize the time series accordingly. 
 Consider an image: it is a NxN object. To feed it to the NN you flatten it: for example the "image" 
 
|  |  |  |  |  |
|--|--|--|--|--|
|A1|A2|A3|A4|A5|
|B1|B2|B3|B4|B5|
|C1|C2|C3|C4|C5|
|D1|D2|D3|D4|D5|
|E1|E2|E3|E4|E5|

would be passed to the NN as 

       A1|A2|A3|A4|A5|B1|B2|B3|B4|B5|C1|C2|C3|C4|C5|D1|D2|D3|D4|D5|E1|E2|E3|E4|E5
       
So now your time series is a vector of "t" times and a vector of "f" fluxes that can  be arranged as 
       
        
       t1|t2|t3|t4...
       f1|f2|f3|f4...


you can pass it as 

         input = t1|t2|t3|t4....f1|f2|f3|f4
         
and ask it to predict 

         output = t1_out|t2_out|t3_out|t4_out....f1_out|f2_out|f3_out|f4_out
         
         
then when you want to see if your autoencoder reproduces the time series you will compare
```
pl.plot(t, f) # the original time series
```
with 
```
pl.plot(output[:N], output[N:])
```
Or you can think of some other creative reorganizaion. To be honset this is unlikely to work well: you are asking the model to predict twice as many thigs as necessary here: the time and the flux! but as I said, you are free to approach this homework with an adventurous spirit. 

4: create an autoencoder where the middle layer is smaller than the original size of the time series (or set of time series if you work with all colors at once). After training the NN on the training data extract the bottleneck layer of your neural network. This will be a set of N numbers (where N depends on the choice you made when you generated the model architecture). Those numbers should be used as N features to be fed to the same random forest classifier you created in HW7 (which you will need to retrain).  You should include the same features you had used in HW7 **plus he  the new N feature** and then print the accuracy, and the classification matrix. Are there any improvements?


**note on grading** 

*I will grade this personally.* You can make whatever adventurous choices you want for this, BUT, all the choices you make have to be explained in writing: why did you organize the data as you did? what are the pros and cons of that choice? Why is your autoencode architecture what it is? what is the optimization chosen, the loss function, why? what is the improvement if any? did it help improve the worse classes or did the improvement only happen in classes that were already ok?


**deadline** 

Sunday 10 12-midnight. The deadline is extended from the traditional Wednesday because we did not get to play with autoencoders in groups in class on thursday. You will do that next tuesday. To be clear: the deadline is extended to give you time to practice writing an autoencoder during class before you have to do it in the homework, I strongly suggest that you prepare your data and make the choices you need to make about data organization by wednesday. If you want to interpolate the data you should have that done sooner, ratehr than later. Training NNs can be very slow! if you leave this to the last minute you may not be able to finish. Once you work on autoencoders in class on tuesday you should be all set up and ready to create your autoencoder and train it to finish the homework!

Next week a (possibky short) final assignment on recurrent neural networks will also be assigned






