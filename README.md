# 046211 - STOP FOR THE WIN 
winter 2025
# Topics 
- Introduction
  - project objective
  - Motivation
  - Previous Work
  - Strategy  
- Design
  - strcutre
  - Data
  - Prediction Model
  - Strategy Model
- Results
  - Prediction Model
  - Strategy Model
  - Comprasion to previous work
- Conclusion
- Future work
- How to run
- Ethics statement
- References
  # Introduction
## Project Objective 
This project aims to develop an optimized pit stop strategy for Formula 1, to improve final 
position. Pit stops are crucial race moments where cars receive quick servicing, such as tire 
changes or mechanical fixes. Our focus is on enhancing pit stop efficiency to improve race 
outcomes. 
## Motivation 
With 10 teams operate under different budgets, Formula 1 is not always a level playing field. 
Our model aims to bridge these gaps by providing tailored pit stop strategies. Mercedes 
already uses AI for race strategy, proving the potential of such models. Additionally, 
broadcasters and fans can gain valuable insights into pit stop decisions. 
## Previous Work 
We build on prior research, particularly Fatima et al. (2023), who introduced Deep-Racing, 
the first motorsports Embedded Deep Neural Network (EDNN) for predicting driver rankings 
and optimal pit stops. We adopted their approach of categorizing data into distinct types and 
embedding each section accordingly, starting from their architecture and adapting it to our 
specific objectives. 
## Strategy  
Pit stop strategy depends on various factors like tire wear, compound selection, and race 
events (e.g., safety car interventions after accidents). Since some variables like mechanical 
issues are unpredictable, we focus on tire-related strategies, analysing: 
1. The number of pit stops per race 
2. The tire compounds used 
3. The laps on which pit stops occur  
This approach aims to achieve a targeted and effective pit stop strategy model.
# Design
## Structure
The goal is to develop a pit stop strategy generation model (referred to as the "strategy model") that leverages a pre-trained position improvement model (the "prediction model"). 
![image](https://github.com/user-attachments/assets/4e4ce3e9-3395-46f3-8f46-e8ec0fb9319d)

## Data
In Formula 1, a wealth of data is available at both the race and lap levels. However, because 
our focus is on tire-related pit stop strategies, we assumed that per-race data would suffice, 
and that the additional complexity of per-lap data was unnecessary. 
###  Sorting the Data
We began by identifying key variables such as race location, weather, driver and team 
identities, grid positions, and pit stop details (number, tire compounds, and pit stop laps). 
Using the fastf1 Python library, we collected data from the 2018–2024 seasons and filtered 
out entries with unusually long pit stops (indicating non-tire-related stops). This resulted in a 
dataset of approximately 2,600 samples, albeit with some imbalance due to similar strategies.
###  Preprocessing the Data
Standard preprocessing steps like feature scaling and encoding categorical data were applied. 
Additionally, we stratified the data to create balanced subsets and used linear interpolation to 
generate extra training samples, ensuring better model generalization. 
## Prediction Model
This model is a multi-input deep learning system that integrates categorical, numerical, and 
sequential data using embeddings, BiLSTMs, and attention mechanisms. It processes pit stop 
and tire sequences separately before merging all inputs through dense layers to generate the 
final prediction, utilizing a hybrid MSE-L1 loss function with epoch-based warm-up. 
![image](https://github.com/user-attachments/assets/a0e70516-bb6c-419d-98a6-86ed721ac7f5)

## Strategy Model
The model encodes categorical and numerical inputs, combining them into a unified 
representation. It then branches into three heads to predict pit stop likelihood, timing, and tire 
choice. Training uses adaptive optimization, mixup augmentation, and a cyclic learning rate, 
optimizing position changes while enforcing stint length, tire diversity, and pit timing 
constraints. 
![image](https://github.com/user-attachments/assets/091f1ca7-ab99-421b-8257-a7ce7468560d)
# Results
##  Prediction Model
![image](https://github.com/user-attachments/assets/527081b0-b6c9-4c50-a456-601500255614)
The model demonstrates moderate accuracy, but there is room for improvement. The RMSE for both the test and validation sets remains around 4.
## Strategy Model
![image](https://github.com/user-attachments/assets/0ab0c694-5d08-4682-a6d0-4395526b95b2)
The loss function does not exhibit a downward trend, suggesting that the model struggles to optimize its strategy over time. As a result, position gains remain stagnant.
![image](https://github.com/user-attachments/assets/6cec0327-17e5-46e1-bcb0-430996932bdf)
The generated strategies favour medium tire compounds and lack diversity. While the model reasonably predicts the number of pit stops—where more stops generally indicate a suboptimal strategy—the timing of these stops is overly clustered around the middle of the race.
Although the generated strategies align somewhat with real-world data, their limited variation is a concern. Furthermore, the position prediction model’s inaccuracies make it difficult to reliably assess the effectiveness of these strategies.
## Comprasion to previous work
Fatima et al. (2023), whose work we built upon, focus on predicting final race positions rather than position improvement, achieving superior results with an RMSE of 2 on the test set. Their strategy generation approach, like ours, primarily considers common pit stop strategies and addresses only tire-related pit stops. A key distinction is their use of per-lap data, enabling both pre-race and real-time strategy analysis, whereas our model is limited to pre-race strategy planning. However, their article does not include results for their strategy generation model.
# Conclusion
## Prediction Model
The model has decent performance, yielding reasonable predictions in most cases. Data preprocessing, augmentation, and the use of appropriate loss functions have further improved the predictions.
Despite exploring various architectures—including transformers, RNNs, and ensemble models with adaptive weights—the test RMSE remained around 4. While further architectural improvements may help, data quality appears to be the primary limiting factor.
## Strategy Model
The strategy model generates valid but overly conservative strategies, favouring standard pit stop patterns. Removing constraints led to unrealistic results, such as excessive pit stops and rare tire choices, indicating overfitting to infrequent scenarios rather than effective generalization.
A more adaptive architecture is needed to generate realistic and diverse strategies that better reflect real-world decision-making.
# Future work
Our research lays a strong foundation for AI-driven motorsport strategy modelling, but key improvements are necessary:
•	Data: Pit stop strategies are influenced by unpredictable events such as accidents and driver positioning, which our model does not account for. Future improvements should prioritize data preprocessing and incorporate per-lap data for a more accurate representation of race dynamics.
•	Model Architecture: While position prediction shows promise, fine-tuning is needed. The strategy model requires a fundamental redesign, with reinforcement learning as a promising direction for continuous strategy optimization.
As AI-driven strategy models gain traction in motorsports—such as those used by Mercedes—and as more data is collected, our approach has the potential to become even more accurate and valuable with further refinement.
# How to run
## Clone the repository:
```
git clone https://github.com/yanirVZ/deep_project
``` 
## Run the models 
### With existing data sets
RUN PitStopStartegy_Model for pitstops strategy generation
RUN Pos_Improve_Pred_Model for position improvement predictions
The models are saved to checkpoints directory
### With new data sets 
RUN obtain_data to extract the features and targets using fastf1 libary. Change the variable "year" to the desired year, extracting data from several years in one run is unstable.
RUN process_data to combine the features and targets from different years and create mappings of the categorical features. 
The data sets are saved to data directory, and the mappings of the categorical features are saved to data/mappings directory.
RUN PitStopStartegy_Model for pitstops strategy generation
RUN Pos_Improve_Pred_Model for position improvement predictions
The models are saved to checkpoints directory
# Ethical statement 
Stakeholders: Formula 1 teams, fans, broadcasters and commentators.
Implications: Teams can optimize performance and race strategies, though over-reliance may limit human adaptability. Broadcasters and commentators can enhance analysis, enriching the fan experience with deeper insights.
Ethical Considerations: Unequal access to the model could create unfair advantages. To maintain fairness, it should be either universally accessible or regulated to preserve the sport’s integrity. 
# References
Fatima, S. S. W., & Johrendt, J. (2023). Deep-Racing: An Embedded Deep Neural Network (EDNN) Model to Predict the Winning Strategy in Formula One Racing. International Journal of Machine Learning, 13(3), 97-103. doi: 10.18178/ijml.2023.13.3.1135









