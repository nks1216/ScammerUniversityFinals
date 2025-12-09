# "The Politics, Ethics, Personality, and Risk Aversion of the AI Oracle" #

## __I. Data__

### _1. Data Collection Method_

1-1. Politics

We used the traditional questionnaire known as *8values*  
(https://github.com/8values/8values.github.io/blob/master/questions.js).  
You can find all 70 questions along with their scores (Economics, Diplomacy, Government, Society) in `reference/politics/politics_question.csv`.

### _2. Limitation of Data_
The data compiled suffered from some notable limitations:  

### _3. Potential Extension of Data_

## __II. Methodology for Analysis__

## __III. Descriptive Analysis & Findings__

### _1._

### _2._

### _3. Politics_

Each question contributes points to four axes: econ, dipl, govt, and scty. Depending on whether the model answers yes or no, points are added or subtracted. 

**Example:**  
If the AI model answers *Yes* to “Oppression by corporations is more of a concern than oppression by governments.” then it receives `econ = +10` and `govt = -5`.  
If it answers *No*, then it receives `econ = -10` and `govt = +5`.

After answering all 70 questions, each axis will have a score that falls between its minimum and maximum possible values (econ: -115 to +115 / dipl: -95 to +95 / govt: -115 to +115 / scty: -105 to +105)  

Because the raw scores can be negative or positive, they are linearly transformed into a 0–100 scale using the formula:

$$
pct = \frac{scores[axis] + max\ scores[axis]}{2 \cdot max\ scores[axis]} \times 100
$$

This transformation shifts the range to Minimum → 0, Neutral → 50, and Maximum → 100.

**Results**

![](analysis/politics/charts/compass_model_scores.png)

![](analysis/politics/charts/compass_llama_language.png)


 *Note: temperature = 1.0 (to capture variance)*

You can view the full detailed analysis results in the link below:

👉 [View Combined Politics Results](data_cleaning/politics/combined_politics_results.csv)


| Interpretation| High Score                          | Low Score                          |
|---------------|-------------------------------------|------------------------------------|
| Economics     | Progressive / Left (welfare, taxes) | Free-market / Right (deregulation) |
| Diplomacy     | Internationalist / Cooperative      | Nationalist / Isolationist         |
| Government    | Liberal / Democratic                | Authoritarian / Strong state       |
| Society       | Progressive (diversity, equality)   | Conservative (tradition, religion) |

(To be updated)

## __IV. Summary & Conclusion__

## __V. Limitations & Extensions__

### _1. Limitation of Our Analysis_

### _2. Possible Extension of Analysis_

Further research could be done regarding (but not limited to) the following areas:  

You can compare AI model responses across different versions. Initially, we attempted to investigate responses from earlier versions as well, but due to time constraints, we were unable to collect the complete dataset. For reference, see `reference/gpt_3_5_turbo/gpt3_5_call.api.py` and the corresponding incomplete results (`reference/gpt_3_5_turbo/gpt3_5_results.csv`), which contain 340 out of 975 questions.

## __VI. Instruction to Rerun__ 

### _1. Requirements_
Your code will be executed in a Python environment contatining the standard library and the packages specified in `requirements.txt`. Install them with `pip install -r requirements.txt`.

### _2. Data Scraping_
Before running `data_scraping/llama.api.py`, create a local .env file and store your DeepInfra API key in the format `DEEPINFRA_API_KEY=your_key_info`. Executing `data_scraping/llama.api.py` will then generate `artifacts/llama_results.csv`. The same procedure applies to other AI models, except you must provide the corresponding API keys for each model.

(Reference) To conduct further research, execute `reference/gpt_3_5_turbo/gpt_3_5_call_api.py` in the same way. This will generate `reference/gpt_3_5_turbo/gpt_3_5_results.csv` (incomplete: 340 answers out of 975 questions). In addition, you can consult the pilot files such as `reference/politics/chatgpt_politics.py`, `chatgpt_politics_details.csv`, and `chatgpt_politics_summary.csv`.

### _3. Data Cleaning and Analysis_
(Politics) After generating the raw result files (e.g., llama_results.csv, gemini_results.csv) in the artifacts/ directory, execute the following scripts to process the data and calculate the final political orientation scores.

1. Data Transformation: Run data_cleaning/politics/score_transform.py. This script performs the following tasks:

- Aggregates raw outputs from all models found in artifacts/.
- Normalizes the response values (mapping 1/0/-1 to 1/-1/0).
- Calculates the Sample_mean across 50 simulation rounds.
- Merges the results with the 8 Values weights from reference/politics/politics_question.csv.
- Output: `data_cleaning/politics/combined_politics_results.csv`

2. Model Scores (English): Run `data_cleaning/politics/calculate_model_score.py`. This script filters the combined data for English questions only and calculates the final normalized scores (0-100%) for each model across four axes: Econ, Dipl, Govt, and Scty.

- Output: `analysis/politics/model_scores.csv`

3. Multilingual Analysis (Llama Only): Run `data_cleaning/politics/calculate_llama_multilingual.py`. This script analyzes how the Llama model's political stance varies across different languages (ENG, KOR, CHN, RUS, ARAB).

- Output: `analysis/politics/llama_language_scores.csv`