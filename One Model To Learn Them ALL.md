- One Model To Learn Them All - https://arxiv.org/abs/1706.05137

The work designs a single model that aims to learn from multiple tasks (Speech, Image, Parsing, Translation) simultaneously. 
Being ambitious enough, the title already stimulates readers to take a look by itself. What components should such a model include? Three basic ingredients are here:

1. Convolutional blocks with depthwise separable convolutions plus dilation (see this for an introduction https://github.com/hoangcuong2011/Good-Papers/blob/master/Depthwise%20Separable%20Convolutions%20for%20Neural%20Machine%20Translation.md)
2. Attention blocks with multi-head dot-product attention mechanism (see this for an introduction https://github.com/hoangcuong2011/Good-Papers/blob/master/Attention%20Is%20All%20You%20Need.md). The timing signals are also included, but note that they are different to the ones proposed in the original work (Attention Is All You Need - https://arxiv.org/abs/1706.03762). I am not so whether this has any benefit or not.
3. Mixture of Experts blocks with sparsely-gated mixture-of-experts layers (see this for an introduction https://github.com/hoangcuong2011/Good-Papers/blob/master/Outrageously%20Large%20Neural%20Networks:%20The%20Sparsely-Gated%20Mixture-of-Experts%20Layer.md)

Those are some among the latest advancements in Deep Learning as of June 2017. It was quite surprising to me that the work did not include self-attention, though.

How to combine them together to have a framework that can work for all different tasks? The paper has three main important
contributions:

It presents a MultiModel framework that consists of 3 parts. The encoder that processes inputs, 
the mixer part that mixes the encodeded inputs with previous generating outputs, and
a decoder that processes the inputs and the mixture to generate the output.

More specifically, the encoder consists of 6 repeated conv blocks, but with a mixture-of-experts layer in the middle. The mixer consists of an attention block and 2 conv blocks. The decoder
consists of 4 blocks of convs and attention, with a mixture-of-experts layer in the middle. The convs in the mixer and decoder are padded on the left so that
we ignore information in the future (a standard technique). Again, I am not so sure why they did not apply self-attention as well, because I believe it may help a lot.

The second contribution of the paper is to show how to process different types of inputs. Multimodel has four different modality nets for text, images, audio and categorical data.

The last main contribution of the paper is to show that training the model jointly on all tasks simulatenously sometimes can help (Table 2) (Table 4 also shows some other result, which is not particularly interesting I think).

Specifically, this is the case of speech and parsing, where the joint training model helps improve over the model trained separately just on a single task - very nice!

As a side note it is not clear to me how they train the model jointly. I guess it is "round-robin" strategy: the model is trained on the first task, and then the second task, the third task, and so on.

While the work is definitely exciting, everything is still far behind what was advertised by the catchy title (I think many people have a problem with this title!) The framework gives a performance that is is substantially behind SOTA (Table 1).  Admittedly when the baseline is not that strong, it is quite hard to fully convince readers any statement.

The authors commented they didn't tune well the model, and I believe all of us are eager to wait (and hope) for a *much* better result from the authors in near future.

As a side note, I was wondering whether the model should be tuned simultaneously over all the tasks or not. That would be great, but very hard to do and I am not sure it would be helpful (I doubt that is even harmful).
