# On Animal Outcomes in Austin Shelters

This survey evaluates the potential to predict adoption outcomes for animals entering the Austin Animal Center shelter system and examines the implications of these findings.

### Table of Contents:

- Part I: Summary of Findings
- Part II: File Directory
- Part III: Datasets Used
- Part IV: Conclusion

## Part I: Summary of Findings

Pet shelters by their nature face enormous pressure to care for and to effect the ultimate wellbeing of animals brought into their care, and to save as many animals as possible. One challenge this creates is capacity management, as shelters have limited space and resources to allocate for this purpose. For these reason, a thorough understanding of the shelter system and of outcome likelihoods for the animals in question is necessary for the fulfillment of its central mission, because it allows a shelter to manage capacity efficiently, both to maximize the potential for adopting out certain animals quickly and for better planning to help as many "hard cases" as circumstances allow.

This project seeks primarily to meet this need through the development of the strongest model possible in terms of its predictive power as related to an animal's ultimate outcome (i.e. whether or not an animal will be adopted). With such a model fine-tuned to the needs of this or that shelter, how many more animals might saved and found forever homes?

The process began with two datasets taken from the Austin Animal Center, one for intake information (or an animal's information upon being brought into the shelter system) and one for outcome information (information pertaining to an animal's being adopted, returned to owner, euthanized, and so on). The first step of preprocessing was to omit problematic/erroneous entries from each dataset. Then columns entered as strings were converted where possible to more model-friendly numeric values, and many features were engineered to experiment with anything that might prove useful for the predictive model. The final step was to merge the Intakes and Outcomes datasets, so that each stay of an animal at last consisted of a single entry in the final dataframe.

The modeling process itself involved a great deal of exploration. The project team explored the possibility of modeling the dataset to predict an animal's duration of stay, as such a capacity would indeed be useful for any shelter--but these attempts were unsuccessful, resulting in wide margins of error. The team had far greater success modeling for classification, both multiclass (an animal's final outcome across a variety of discrete possibilities) and binary--simply, predicting based on the available information whether or not an animal will be adopted. 

Largely because focus on this binary possibility of outcomes aligns closely with the concerns and needs listed above for animal shelters in general, this became the primary model for this survey. Ultimately, a simple Gradient Boost Classifier model showed the strongest results in terms of accuracy of any that were tried, performing well above baseline. The final model exists in six different versions, and the version used depends on the intake type for each entry (whether or not the animal is brought in by owner surrender, as a stray, etc.) The final StreamLit app filters for this value and applies the correct version of the model based on the information a user enters.

## Part II: File Directory

|File or Folder|Description|
|---|---|
|app/|all things related to the final StreamLit app, including subfolders for the pickled models and the app file itself|
|appendicies/appendix_01_preprocessing|includes notebooks primarily related to cleaning the data, exploratory analysis, and engineering features for modeling|
|appendicies/appendix_02_preprocessing|includes a comprehensive notebook of all six versions of the final model, replete with accuracy scores, confusion matrixes, and more detailed markdown|
|datasets/|includes the data used for this project, from its initial version to its final version, in a collection of progressively more usable .csv files|
|main.ipynb|the project's primary Jupyter notebook, providing concise explanations of its approach and findings (with code and graphs)|
|presentation.pdf|a PDF version of the slide presentation given in conjunction with the completion of this project|
|README.md|a summary of the project's aims, findings, data dictionary, and conclusions|
|scrap/|a catchall folder that includes much of the scratch work done over the course of the project by all members of the team, all in its original form across a collection of Jupyter notebooks|

## Part III: Datasets Used

- [`Austin_Animal_Shelter_Intakes.csv`]('datasets/Austin_Animal_Shelter_Intakes.csv'): the initial Intakes dataset taken from the Austin Animal Center
- [`Austin_Animal_Shelter_Outcomes.csv`]('datasets/Austin_Animal_Shelter_Outcomes.csv'): the initial Outcomes dataset taken from the Austin Animal Center
- [`intakes_initial.csv`]('datasets/intakes_initial.csv'): the Intakes dataset after preliminary cleaning, before the merge with Outcomes
- [`main`]('datasets/main.csv'): the first version of the merged dataset, used as the foundation for much of the team's scratch work and the source for the final dataset used for modeling
- [`model_frame.csv`]('datasets/model_frame.csv'): the dataset used for modeling
- [`outcomes_initial.csv`]('datasets/outcomes_initial.csv'): the Outcomes dataset after preliminary cleaning, before the merge with Intakes

Below is a list of relevant features included in `model_frame.csv` as they relate to the final predictive model:

|Feature|Type|Dataset|Description|
|---|---|---|---|
|age_upon_intake|int|model_frame.csv|the age of the animal in question in years at the time of entering the shelter|
|age_type|string|model_frame.csv|the age of the animal in question, bracketed into five different age ranges|
|animal_type|string|model_frame.csv|the species of the animal in question|
|breed|string|model_frame.csv|the breed of the animal in question|
|color|string|model_frame.csv|the color of the animal in question|
|day_in|string|model_frame.csv|the day of the week that the stay for the animal in question began|
|days_in_shelter|int|model_frame.csv|the duration of an animal's stay in the shelter at the time of outcome|
|intake_condition|string|model_frame.csv|details related to an animal's condition at the time of being brought into the shelter|
|intake_type|string|model_frame.csv|details related to thecircumstances surrounding an animal's entering the shelter|
|is_named_in|int|model_frame.csv|binarized column reflecting whether or not an animal has a name upon entering the shelter|
|is_neutered|string|model_frame.csv|column reflecting whether or not an animal is spayed or neutered at the time of outcome|
|mix|int|model_frame.csv|binarized column reflecting whether or not the animal is a mixed breed|
|month_in|int|model_frame.csv|integer representation of the month during which an animal is brought into the shelter|
|outcome_type|string|model_frame.csv|categorical column reflecting an animal's ultimate outcome for that stay in the shelter|
|prev_adoption|int|model_frame.csv|the number of times an animal in question has been taken into the shelter and adopted|
|prev_ret_to_owner|int|model_frame.csv|the number of times an animal in question has been taken into the shelter and then returned to the owner|
|prev_transfer|int|model_frame.csv|the number of times an animal in question has been taken into the shelter and then transferred|
|sex|string|model_frame.csv|column reflecting whether an animal is male or female|

## Part IV: Conclusion

Ultimately, the project team was successful in crafting a model that performs well above the baseline when predicting whether or not an animal is adopted. Key variables include duration of stay and animal age. As the duration of stay increases, so too does likelihood of adoption. As age increases, adoption likelihood decreases.