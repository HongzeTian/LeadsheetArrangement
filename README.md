# LeadsheetArrangement 自動簡譜編曲
[Lead Sheet Arrangement](https://liuhaumin.github.io/LeadsheetArrangement/) is a task to automatically accompany the existed or generated lead sheets with multiple instruments. The rhythm, pitch notes, onsets and even playing style of different instruments are all learned by the model. 

We train the model with Lakh Pianoroll Dataset (LPD) to generate pop song style arrangement consisting of bass, guitar, piano, strings and drums.

Sample results are available
[here](https://liuhaumin.github.io/LeadsheetArrangement/results).

## Papers

__Lead sheet generation and arrangement by conditional generative adversarial network__<br>
Hao-Min Liu and Yi-Hsuan Yang,
to appear in *International Conference on Machine Learning and Applications* (ICMLA), 2018.
[[arxiv](https://arxiv.org/abs/1807.11161)]

__Lead Sheet Generation and Arrangement via a Hybrid Generative Model__<br>
Hao-Min Liu\*, Meng-Hsuan Wu\*, and Yi-Hsuan Yang
(\*equal contribution)<br>
in _ISMIR Late-Breaking Demos Session_, 2018.
(non-refereed two-page extended abstract)<br>
[[paper](https://liuhaumin.github.io/LeadsheetArrangement/pdf/ismir2018leadsheetarrangement.pdf)]
[[poster](https://liuhaumin.github.io/LeadsheetArrangement/pdf/ismir-lbd-poster_A0_final.pdf)]

__Lead sheet and Multi-track Piano-roll generation using MuseGAN__<br>
Hao-Min Liu, Hao-Wen Dong, Wen-Yi Hsiao and Yi-Hsuan Yang,
in *GPU Technology Conference* (GTC), 2018.
[[poster](https://liuhaumin.github.io/LeadsheetArrangement/pdf/GTC_poster_HaoMin.pdf)]

## Usage

```python
import tensorflow as tf
from musegan.core import MuseGAN
from musegan.components import NowbarHybrid
from config import *

# Initialize a tensorflow session

""" Create TensorFlow Session """
with tf.Session() as sess:
    
    # === Prerequisites ===
    # Step 1 - Initialize the training configuration        
    t_config = TrainingConfig
    t_config.exp_name = 'exps/nowbar_hybrid'        

    # Step 2 - Select the desired model
    model = NowbarHybrid(NowBarHybridConfig)
    
    # Step 3 - Initialize the input data object
    input_data = InputDataNowBarHybrid(model)
    
    # Step 4 - Load training data
    path_x_train_bar = 'tra_X_bars'
    path_y_train_bar = 'tra_y_bars'
    input_data.add_data_sa(path_x_train_bar, path_y_train_bar, 'train') # x: input, y: conditional feature
    
    # Step 5 - Initialize a museGAN object
    musegan = MuseGAN(sess, t_config, model)
    
    # === Training ===
    musegan.train(input_data)

    # === Load a Pretrained Model ===
    musegan.load(musegan.dir_ckpt)

    # === Generate Samples ===
    path_x_test_bar = 'val_X_bars'
    path_y_test_bar = 'val_y_bars'
    input_data.add_data_sa(path_x_test_bar, path_y_test_bar, key='test')
    musegan.gen_test(input_data, is_eval=True)

```
