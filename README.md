# pixel-rnn
An explanation of the generative modelling concept of Pixel RNN's

Link to paper : https://arxiv.org/abs/1601.06759

When I first read into the concept of generative models with the ability to generate tractable densities, I came across the concept of Pixel RNN's proposed by Van den Oord, et al.

I found that some of the results mentioned in the paper were a bit hard to understand at first glance and thus chose to write a post about how I got to understanding the small components involved in a Pixel RNN.

Before we start it is absolutely crucial that you have a good understanding of the concept of RNN's and also more specifically LSTM's. 
Here is an excellent post explaining the math behind the BPTT algorithm : http://willwolf.io/2016/10/18/recurrent-neural-network-gradients-and-lessons-learned-therein/
Here is perhaps one of the best explanations of an LSTM and the various gates involved in it : http://colah.github.io/posts/2015-08-Understanding-LSTMs/ (Note that this does not explain why it solves the vanishing/exploding gradients problem normally faced in a primitive RNN)

Another important point is that initially RNN's and LSTM's were usually applied to one dimensional sequences. Even if RNN's were applied to multi-dimensional data, it first had to be converted to a one dimensional sequence, thus resulting in a loss of spatio-temporal information. This would be a crucial loss of information,(lets take an example) given that pixels in an image are usually influenced by their neighboring pixels.
So how could this concept of RNN's be applied to multi-dimensional data without loss of spatio-temporal information, to solve this problem Alex Graves,et al came up with an intuitive idea as given in their paper : https://arxiv.org/pdf/0705.2011.pdf
Now that you have understood Multi-dimensional RNN's, it is easy to think of multi-dimensional LSTM's ,imagining the LSTM cell as an RNN black-box. This idea was implemented in the following paper : https://arxiv.org/pdf/1506.03478.pdf

Now, if you have understood the above concepts and also have an idea of what context/memory in an RNN/LSTM is, it is fairly intuitive to understand why RNN / LSTMS's can be used to represent complex probability distributions over images as mentioned in the Pixel RNN paper.

Another crucial concept required for a better understanding of this paper is the idea of Convolutional LSTM's(ConvLSTM), do not confuse this with the architecture where the FC layers at the end of a generic ConvNet are replaced with an RNN/LSTM.
The idea is elucidated in the paper : https://arxiv.org/pdf/1506.04214.pdf and is better explained in this : http://vision.ouc.edu.cn/valse/slides/20160323/VALSE-Xingjian.pdf
This was the reddit question that prompted me to go in this direction : https://www.reddit.com/r/deeplearning/comments/6qa1x3/pixel_rnn_row_lstm_explanation_in_simplest_manner/

Now, if you have a basic understanding of how ConvLSTM's work, understanding the concept of RowLSTM's will be a breeze.
It is mentioned in the paper that the RowLSTM captures a triangular context, lets see why.
The concept of a RowLSTM is to take a whole pixel row of an image as input, rather than pixel by pixel as is usually done.
Now k * 1 masked convolutions are applied on this row to generate a new predicted row.
Lets take the example where k = 3, as most of you might have deciphered by now, the number of pixels predicted in the next row will be the number of convolutions applied on the input row. This can be easily calculated using the generic formula which is used in Conv Nets.
Note that although the stride of the convolutions has not been mentioned in the paper, I am pretty sure it is "1". Also in the PixelRNN paper although not explicitly mentioned, no padding is used. Now if you plug in these values in the convolution formula, the number of convolutions and hence the number of predicted pixels will be (k - 3 + 1) i.e (k - 2).(Note that it is 2 less than the original for any k)
Thus as can be easily inferred from above, the 2 pixels that are always left out and hence not predicted using the original row are the 2 pixels at the corners. Now if this convolution is applied on consequent rows, you can visulaize that with each subsequent row 2 pixels at the corners are left out and thus for a given pixel, a triangular context is captured.
Now as for the forward propogation formulas of the RowLSTM, note that the authors mention 4 values corresponding to the 4 gates of an LSTM and also mention that the sigma function is different for computing different gates (sigmoid for 3 and tanh for the remaining one) as is mentioned in the multi-dimensional LSTM paper above. The rest of the formula is just how an LSTM is implemented as can be seen in the Colah blog link given above.
Thus, the RowLSTM's capture only a triangular context and fail to capture the entire available context.



TO BE CONTINUED

Note: I understand that in some places I have just given links rather than adding my own explanations, so my next objective will be to give detailed explanations once I am done explaining the whole paper.



