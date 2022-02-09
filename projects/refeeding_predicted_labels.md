## Iterated Training With Self-labeled Data 

### Problem

Jurian Kahl, a friend of mine, and I explored the idea of training a neural network with data that has been labeled by itself. This idea may be useful when you have a large amount of data of which, however, only a small portion has been labeled.
It is expected that not enough predictions are correct when the model has only been trained with the small amount of initially labeled data. Therefore, we decided on a "democratic" bagging approach using an ensemble of multiple models: When all (or most of the) models agree on one label, we can say with some confidence that this label is probably correct. Therefore we then add those points the models agree on with their predicted (possibly incorrect) labels to our training data set. 

### Methodology

| ![64 by 64 pixel handwritten digit 0](images/refeeding_predicted_labels/MNIST_example_digit_0.png) | 
|:--:| 
| *example MNIST handwritten digit* |

We decided on using the <a href="http://yann.lecun.com/exdb/mnist/" target="_blank" rel="noopener noreferrer">MNIST</a> data set of handwritten digits and, for simplicity, an ensemble of three (structurally equivalent) convolutional neural networks:

```python
def get_model_conv_nn(
    input_shape,
    dropout = 0.2,
    activation = 'relu',
    solver = 'SGD',
    kernel_size = (3, 3),
    pool_size = (2, 2),
    padding = 'same',
    output_activation = "softmax",
    num_outputs = 10,
    filters = 16
):
    model = tf.keras.Sequential()

    # convolutional layers
    model.add(Conv2D(filters=32, kernel_size=kernel_size, input_shape=input_shape, padding=padding))
    model.add(Activation(activation))
    model.add(MaxPool2D(pool_size=(2, 2), strides=None, padding='valid'))

    model.add(Conv2D(filters=64, kernel_size=kernel_size, input_shape=input_shape, padding=padding))
    model.add(Activation(activation))
    model.add(MaxPool2D(pool_size=(2, 2), strides=None, padding='valid'))
    # fully connected layers
    model.add(Flatten())
    model.add(Dense(128, activation=activation))

    # output layer
    model.add(Dense(units=num_outputs, name="output", activation=output_activation))

    model.compile(
        optimizer=solver,
        loss=tf.keras.losses.CategoricalCrossentropy(from_logits=True),
        metrics=['categorical_accuracy']
    )

    return model
```

Of the 70,000 data points we used 5% for the initial training phase. Then we trained each model for 20 epochs and predicted the labels of the remaining data points. Afterwards, we add all data points for which all three models agree to the training set and repeat the previous set until they agree on less than 1,000 labels.

### Results



### Conclusion

The above descriped "democratic" approach only works if the involved parties have different "beliefs". To put this into mathematical terms: In parameter estimation, the error of an unbiased estimator is it's variance. An intuition on why one might want to combine different estimators is the following: If you have <div class="math">n</div> estimators <div class="math">\theta_i</div> with the same variance, then

<img src="https://render.githubusercontent.com/render/math?math=Var(\frac{1}{n} \sum_{i=1}^n \theta_i) = \frac{1}{n^2} n Var(\theta_1) = \frac{1}{n} Var(\theta_1)">

if all <div class="math">\theta_i</div> are uncorrelated. However, our models are obviously not uncorrelated as they have the same structure and are trained with the same data.
