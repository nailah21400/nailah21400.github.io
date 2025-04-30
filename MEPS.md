```python
# Importing Google Drive Connection (For Colab use only)
from google.colab import drive
drive.mount('/content/drive', force_remount=True)
```

    Mounted at /content/drive


Research Question: How do insurance coverage, race, and English proficiency affect health outcomes for individuals diagnosed with speech and language disorders?

Primary hypothesis: Individuals with Medicaid or no insurance will report worse health outcomes compared to those with private insurance.

 This project aims to provide the following analysis: Descriptive stats will describe the frequency of speech codes throughout the data set by insurance type, race, and English proficiency and a summary stats of health outcomes; Chi-square test: check relationship between insurance type, race, English proficiency and health outcomes; and ANOVA: compare mean health outcomes by insurance type, race, and English proficiency

## Problem Statement
Research indicates that race, ethnicity, type of insurance, and english proficiency are associated with worse health outcomes, access to healthcare and quality of providers. Little is known about the disparities in health outcomes for individuals with speech, language, and swallowing disorders. This study seeks to investigate potential associations between health outcomes for patients with speech, language, and swallowing disorders based on english proficency, race, and type of insurance.

##Research Question

How do insurance coverage, race, and English proficiency affect health outcomes for individuals diagnosed with speech and language disorders?

**Primary Hypothesis:** Individuals with Medicaid or no insurance will report worse health outcomes compared to those with private insurance.

This project aims to provide the following analyses: Descriptive stats will describe the frequency of speech

1. Descriptive statistics will describe the frequency of speech codes throughout the data set by insurance type, race, and English proficiency and provide a summary of health outcomes.
2.   Chi-square test will see if there is a significant relationship between insurance type, race, English proficiency and health outcomes
3. Analysis of Variance (ANOVA) will compare mean health outcomes by insurance type, race, and English proficiency


## Data Definition
**Medical Expenditure Pannel Survey (MEPS)**

Last Updated: April 14, 2023
https://meps.ahrq.gov/mepsweb/about_meps/survey_back.jsp

The Medical Expenditure Panel Survey (MEPS) is a set of survey questions for individuals, families, and their corresponding medical providers across the United States. Medical employers and insurance company data was also included. This survey was conducted by the Agency for Healthcare Researcha and Quality (AHRQ) and United States Department of Health and Human Services (HHS).

The MEPS collects data on health services used, the frequency of use, and expenditures of the services. The Household Component (HC) gathers interview data from a nationally representative sample of families and indiviudals over a two year period. The Insurance Component (IC) data is on what health care plans  private and public employers offer to their employees. The Medical Provider Component (MCP) is supplemental data on the health care providers, hospitals, physicians, and pharmacies identified by the family and individual participants. The MEPS has data avaialble for 1996-2023. More information about collection methods, background information, and history can be found here: https://meps.ahrq.gov/mepsweb/about_meps/survey_back.jsp
  

## Data Collection
This data was accessed through the HHS-AHRQ MEPS GitHub repository. The repository is accessed directly from Colab using different Python libraries and functions.

The datasets used for this analysis were the following 2022 categories:


*   Full Year Consolidated (fyc22)
*   Medical Conditions (cond 22)



## Examine the Dataset


```python
##Import the libraries
import numpy as np                  # Scientific Computing
import pandas as pd                 # Data Analysis
import matplotlib.pyplot as plt     # Plotting
import seaborn as sns               # Statistical Data Visualization

# Let's make sure pandas returns all the rows and columns for the dataframe
pd.set_option('display.max_rows', None)
pd.set_option('display.max_columns', None)

# Force pandas to display full numbers instead of scientific notation
pd.options.display.float_format = '{:.0f}'.format

# Library to suppress warnings
import warnings
warnings.filterwarnings('ignore')
```


```python
# Importing Google Drive Connection (For Colab use only)
from google.colab import drive
drive.mount('/content/drive', force_remount=True)
```

    Mounted at /content/drive



```python
import zipfile
import pandas as pd

#View the Medical Conditions(cond22) data
# Open the zip file
with zipfile.ZipFile("/content/drive/MyDrive/MEPS/cond22.xlsx.zip", 'r') as zip_ref:
# Get the name of the Excel file within the zip
    excel_file_name = zip_ref.namelist()[0]
# Extract the Excel file to memory
    with zip_ref.open(excel_file_name) as extracted_file:
# Read the Excel file using pandas
        path = pd.read_excel(extracted_file)
```


```python
#Veiw the first 7 rows of the Medical Conditions data frame
cond22 = pd.DataFrame(path)
cond22.head(7)
```





  <div id="df-d39728b8-bdea-4cb4-899f-d6c8bdb7d645" class="colab-df-container">
    <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DUID</th>
      <th>PID</th>
      <th>DUPERSID</th>
      <th>CONDN</th>
      <th>CONDIDX</th>
      <th>PANEL</th>
      <th>CONDRN</th>
      <th>AGEDIAG</th>
      <th>CRND1</th>
      <th>CRND2</th>
      <th>CRND3</th>
      <th>CRND4</th>
      <th>CRND5</th>
      <th>CRND6</th>
      <th>CRND7</th>
      <th>CRND8</th>
      <th>CRND9</th>
      <th>INJURY</th>
      <th>ACCDNWRK</th>
      <th>ICD10CDX</th>
      <th>CCSR1X</th>
      <th>CCSR2X</th>
      <th>CCSR3X</th>
      <th>CCSR4X</th>
      <th>HHCOND</th>
      <th>IPCOND</th>
      <th>OPCOND</th>
      <th>OBCOND</th>
      <th>ERCOND</th>
      <th>RXCOND</th>
      <th>PERWT22F</th>
      <th>VARSTR</th>
      <th>VARPSU</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2460002</td>
      <td>101</td>
      <td>2460002101</td>
      <td>1</td>
      <td>2460002101001</td>
      <td>24</td>
      <td>1</td>
      <td>25</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>-1</td>
      <td>I10</td>
      <td>CIR007</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>5728</td>
      <td>2082</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2460002</td>
      <td>101</td>
      <td>2460002101</td>
      <td>2</td>
      <td>2460002101002</td>
      <td>24</td>
      <td>1</td>
      <td>68</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>-1</td>
      <td>G45</td>
      <td>NVS012</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>5728</td>
      <td>2082</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2460002</td>
      <td>101</td>
      <td>2460002101</td>
      <td>3</td>
      <td>2460002101003</td>
      <td>24</td>
      <td>1</td>
      <td>25</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>-1</td>
      <td>E78</td>
      <td>END010</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>5728</td>
      <td>2082</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2460002</td>
      <td>101</td>
      <td>2460002101</td>
      <td>4</td>
      <td>2460002101004</td>
      <td>24</td>
      <td>1</td>
      <td>50</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>-1</td>
      <td>E11</td>
      <td>END002</td>
      <td>END005</td>
      <td>-1</td>
      <td>-1</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>5728</td>
      <td>2082</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2460002</td>
      <td>101</td>
      <td>2460002101</td>
      <td>5</td>
      <td>2460002101005</td>
      <td>24</td>
      <td>1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>-1</td>
      <td>-15</td>
      <td>NEO000</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>5728</td>
      <td>2082</td>
      <td>1</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2460002</td>
      <td>101</td>
      <td>2460002101</td>
      <td>6</td>
      <td>2460002101006</td>
      <td>24</td>
      <td>1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>-1</td>
      <td>C34</td>
      <td>NEO022</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>5728</td>
      <td>2082</td>
      <td>1</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2460002</td>
      <td>101</td>
      <td>2460002101</td>
      <td>7</td>
      <td>2460002101007</td>
      <td>24</td>
      <td>1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>-1</td>
      <td>C85</td>
      <td>NEO058</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>5728</td>
      <td>2082</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-d39728b8-bdea-4cb4-899f-d6c8bdb7d645')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-d39728b8-bdea-4cb4-899f-d6c8bdb7d645 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-d39728b8-bdea-4cb4-899f-d6c8bdb7d645');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>


    <div id="df-ca75c09c-3131-4517-813c-1e02b1d76e21">
      <button class="colab-df-quickchart" onclick="quickchart('df-ca75c09c-3131-4517-813c-1e02b1d76e21')"
                title="Suggest charts"
                style="display:none;">

<svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
     width="24px">
    <g>
        <path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z"/>
    </g>
</svg>
      </button>

<style>
  .colab-df-quickchart {
      --bg-color: #E8F0FE;
      --fill-color: #1967D2;
      --hover-bg-color: #E2EBFA;
      --hover-fill-color: #174EA6;
      --disabled-fill-color: #AAA;
      --disabled-bg-color: #DDD;
  }

  [theme=dark] .colab-df-quickchart {
      --bg-color: #3B4455;
      --fill-color: #D2E3FC;
      --hover-bg-color: #434B5C;
      --hover-fill-color: #FFFFFF;
      --disabled-bg-color: #3B4455;
      --disabled-fill-color: #666;
  }

  .colab-df-quickchart {
    background-color: var(--bg-color);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: none;
    fill: var(--fill-color);
    height: 32px;
    padding: 0;
    width: 32px;
  }

  .colab-df-quickchart:hover {
    background-color: var(--hover-bg-color);
    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);
    fill: var(--button-hover-fill-color);
  }

  .colab-df-quickchart-complete:disabled,
  .colab-df-quickchart-complete:disabled:hover {
    background-color: var(--disabled-bg-color);
    fill: var(--disabled-fill-color);
    box-shadow: none;
  }

  .colab-df-spinner {
    border: 2px solid var(--fill-color);
    border-color: transparent;
    border-bottom-color: var(--fill-color);
    animation:
      spin 1s steps(1) infinite;
  }

  @keyframes spin {
    0% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
      border-left-color: var(--fill-color);
    }
    20% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    30% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
      border-right-color: var(--fill-color);
    }
    40% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    60% {
      border-color: transparent;
      border-right-color: var(--fill-color);
    }
    80% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-bottom-color: var(--fill-color);
    }
    90% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
    }
  }
</style>

      <script>
        async function quickchart(key) {
          const quickchartButtonEl =
            document.querySelector('#' + key + ' button');
          quickchartButtonEl.disabled = true;  // To prevent multiple clicks.
          quickchartButtonEl.classList.add('colab-df-spinner');
          try {
            const charts = await google.colab.kernel.invokeFunction(
                'suggestCharts', [key], {});
          } catch (error) {
            console.error('Error during call to suggestCharts:', error);
          }
          quickchartButtonEl.classList.remove('colab-df-spinner');
          quickchartButtonEl.classList.add('colab-df-quickchart-complete');
        }
        (() => {
          let quickchartButtonEl =
            document.querySelector('#df-ca75c09c-3131-4517-813c-1e02b1d76e21 button');
          quickchartButtonEl.style.display =
            google.colab.kernel.accessAllowed ? 'block' : 'none';
        })();
      </script>
    </div>

    </div>
  </div>





```python
#Veiw the bottom 7 rows of the Medical Conditions
cond22.tail(7)
```





  <div id="df-ad63278e-f699-47ca-82f9-4840f4f47ebe" class="colab-df-container">
    <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DUID</th>
      <th>PID</th>
      <th>DUPERSID</th>
      <th>CONDN</th>
      <th>CONDIDX</th>
      <th>PANEL</th>
      <th>CONDRN</th>
      <th>AGEDIAG</th>
      <th>CRND1</th>
      <th>CRND2</th>
      <th>CRND3</th>
      <th>CRND4</th>
      <th>CRND5</th>
      <th>CRND6</th>
      <th>CRND7</th>
      <th>CRND8</th>
      <th>CRND9</th>
      <th>INJURY</th>
      <th>ACCDNWRK</th>
      <th>ICD10CDX</th>
      <th>CCSR1X</th>
      <th>CCSR2X</th>
      <th>CCSR3X</th>
      <th>CCSR4X</th>
      <th>HHCOND</th>
      <th>IPCOND</th>
      <th>OPCOND</th>
      <th>OBCOND</th>
      <th>ERCOND</th>
      <th>RXCOND</th>
      <th>PERWT22F</th>
      <th>VARSTR</th>
      <th>VARPSU</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>83166</th>
      <td>2799700</td>
      <td>101</td>
      <td>2799700101</td>
      <td>3</td>
      <td>2799700101003</td>
      <td>27</td>
      <td>1</td>
      <td>65</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>2</td>
      <td>-1</td>
      <td>I49</td>
      <td>CIR017</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>11060</td>
      <td>2001</td>
      <td>3</td>
    </tr>
    <tr>
      <th>83167</th>
      <td>2799700</td>
      <td>101</td>
      <td>2799700101</td>
      <td>6</td>
      <td>2799700101006</td>
      <td>27</td>
      <td>2</td>
      <td>-1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>2</td>
      <td>-1</td>
      <td>I99</td>
      <td>CIR032</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>11060</td>
      <td>2001</td>
      <td>3</td>
    </tr>
    <tr>
      <th>83168</th>
      <td>2799700</td>
      <td>101</td>
      <td>2799700101</td>
      <td>9</td>
      <td>2799700101009</td>
      <td>27</td>
      <td>2</td>
      <td>-1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>2</td>
      <td>-1</td>
      <td>H57</td>
      <td>EYE012</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>11060</td>
      <td>2001</td>
      <td>3</td>
    </tr>
    <tr>
      <th>83169</th>
      <td>2799700</td>
      <td>101</td>
      <td>2799700101</td>
      <td>10</td>
      <td>2799700101010</td>
      <td>27</td>
      <td>2</td>
      <td>-1</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>2</td>
      <td>-1</td>
      <td>M25</td>
      <td>MUS010</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>11060</td>
      <td>2001</td>
      <td>3</td>
    </tr>
    <tr>
      <th>83170</th>
      <td>2799700</td>
      <td>101</td>
      <td>2799700101</td>
      <td>11</td>
      <td>2799700101011</td>
      <td>27</td>
      <td>3</td>
      <td>-1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>2</td>
      <td>-1</td>
      <td>-15</td>
      <td>INJ049</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>11060</td>
      <td>2001</td>
      <td>3</td>
    </tr>
    <tr>
      <th>83171</th>
      <td>2799700</td>
      <td>101</td>
      <td>2799700101</td>
      <td>12</td>
      <td>2799700101012</td>
      <td>27</td>
      <td>3</td>
      <td>-1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>2</td>
      <td>-1</td>
      <td>Z13</td>
      <td>FAC003</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>11060</td>
      <td>2001</td>
      <td>3</td>
    </tr>
    <tr>
      <th>83172</th>
      <td>2799700</td>
      <td>101</td>
      <td>2799700101</td>
      <td>13</td>
      <td>2799700101013</td>
      <td>27</td>
      <td>3</td>
      <td>-1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>2</td>
      <td>-1</td>
      <td>Z76</td>
      <td>FAC012</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>11060</td>
      <td>2001</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-ad63278e-f699-47ca-82f9-4840f4f47ebe')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-ad63278e-f699-47ca-82f9-4840f4f47ebe button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-ad63278e-f699-47ca-82f9-4840f4f47ebe');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>


    <div id="df-c9369e62-d1e6-4ea2-a03e-878133041b7f">
      <button class="colab-df-quickchart" onclick="quickchart('df-c9369e62-d1e6-4ea2-a03e-878133041b7f')"
                title="Suggest charts"
                style="display:none;">

<svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
     width="24px">
    <g>
        <path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z"/>
    </g>
</svg>
      </button>

<style>
  .colab-df-quickchart {
      --bg-color: #E8F0FE;
      --fill-color: #1967D2;
      --hover-bg-color: #E2EBFA;
      --hover-fill-color: #174EA6;
      --disabled-fill-color: #AAA;
      --disabled-bg-color: #DDD;
  }

  [theme=dark] .colab-df-quickchart {
      --bg-color: #3B4455;
      --fill-color: #D2E3FC;
      --hover-bg-color: #434B5C;
      --hover-fill-color: #FFFFFF;
      --disabled-bg-color: #3B4455;
      --disabled-fill-color: #666;
  }

  .colab-df-quickchart {
    background-color: var(--bg-color);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: none;
    fill: var(--fill-color);
    height: 32px;
    padding: 0;
    width: 32px;
  }

  .colab-df-quickchart:hover {
    background-color: var(--hover-bg-color);
    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);
    fill: var(--button-hover-fill-color);
  }

  .colab-df-quickchart-complete:disabled,
  .colab-df-quickchart-complete:disabled:hover {
    background-color: var(--disabled-bg-color);
    fill: var(--disabled-fill-color);
    box-shadow: none;
  }

  .colab-df-spinner {
    border: 2px solid var(--fill-color);
    border-color: transparent;
    border-bottom-color: var(--fill-color);
    animation:
      spin 1s steps(1) infinite;
  }

  @keyframes spin {
    0% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
      border-left-color: var(--fill-color);
    }
    20% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    30% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
      border-right-color: var(--fill-color);
    }
    40% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    60% {
      border-color: transparent;
      border-right-color: var(--fill-color);
    }
    80% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-bottom-color: var(--fill-color);
    }
    90% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
    }
  }
</style>

      <script>
        async function quickchart(key) {
          const quickchartButtonEl =
            document.querySelector('#' + key + ' button');
          quickchartButtonEl.disabled = true;  // To prevent multiple clicks.
          quickchartButtonEl.classList.add('colab-df-spinner');
          try {
            const charts = await google.colab.kernel.invokeFunction(
                'suggestCharts', [key], {});
          } catch (error) {
            console.error('Error during call to suggestCharts:', error);
          }
          quickchartButtonEl.classList.remove('colab-df-spinner');
          quickchartButtonEl.classList.add('colab-df-quickchart-complete');
        }
        (() => {
          let quickchartButtonEl =
            document.querySelector('#df-c9369e62-d1e6-4ea2-a03e-878133041b7f button');
          quickchartButtonEl.style.display =
            google.colab.kernel.accessAllowed ? 'block' : 'none';
        })();
      </script>
    </div>

    </div>
  </div>





```python
# display the dimensions of the data
cond22.shape
```




    (83173, 33)



The Medical Conditions dataframe features 83,173 rows and 33 columns. There are a total of 2,744,709 expect data points in this dataset.


```python
# Check the basic information about the dataset
cond22.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 83173 entries, 0 to 83172
    Data columns (total 33 columns):
     #   Column    Non-Null Count  Dtype  
    ---  ------    --------------  -----  
     0   DUID      83173 non-null  int64  
     1   PID       83173 non-null  int64  
     2   DUPERSID  83173 non-null  int64  
     3   CONDN     83173 non-null  int64  
     4   CONDIDX   83173 non-null  int64  
     5   PANEL     83173 non-null  int64  
     6   CONDRN    83173 non-null  int64  
     7   AGEDIAG   83173 non-null  int64  
     8   CRND1     83173 non-null  int64  
     9   CRND2     83173 non-null  int64  
     10  CRND3     83173 non-null  int64  
     11  CRND4     83173 non-null  int64  
     12  CRND5     83173 non-null  int64  
     13  CRND6     83173 non-null  int64  
     14  CRND7     83173 non-null  int64  
     15  CRND8     83173 non-null  int64  
     16  CRND9     83173 non-null  int64  
     17  INJURY    83173 non-null  int64  
     18  ACCDNWRK  83173 non-null  int64  
     19  ICD10CDX  83173 non-null  object 
     20  CCSR1X    83173 non-null  object 
     21  CCSR2X    83173 non-null  object 
     22  CCSR3X    83173 non-null  object 
     23  CCSR4X    83173 non-null  object 
     24  HHCOND    83173 non-null  int64  
     25  IPCOND    83173 non-null  int64  
     26  OPCOND    83173 non-null  int64  
     27  OBCOND    83173 non-null  int64  
     28  ERCOND    83173 non-null  int64  
     29  RXCOND    83173 non-null  int64  
     30  PERWT22F  83173 non-null  float64
     31  VARSTR    83173 non-null  int64  
     32  VARPSU    83173 non-null  int64  
    dtypes: float64(1), int64(27), object(5)
    memory usage: 20.9+ MB


No null or missing values were observed in this dataset. However, the number of non-null values does not macth the expected total of entries. The columns titled: 'DUPERSID', 'ICD10CDX', 'CONDIDX', 'IPCOND', 'OPCOND', 'OBCOND', 'HHCOND', and 'PERWT22F' are crucial for analysis.

There is 1 categorical variable, 'ICD10CDX', and 1 float data type 'PERWT22F'. The other variables are all intergers.





## Data Cleaning



```python
# Headers
# Create a list of the columns in the dataset
cond22Columns = cond22.columns
cond22Columns
```




    Index(['DUID', 'PID', 'DUPERSID', 'CONDN', 'CONDIDX', 'PANEL', 'CONDRN',
           'AGEDIAG', 'CRND1', 'CRND2', 'CRND3', 'CRND4', 'CRND5', 'CRND6',
           'CRND7', 'CRND8', 'CRND9', 'INJURY', 'ACCDNWRK', 'ICD10CDX', 'CCSR1X',
           'CCSR2X', 'CCSR3X', 'CCSR4X', 'HHCOND', 'IPCOND', 'OPCOND', 'OBCOND',
           'ERCOND', 'RXCOND', 'PERWT22F', 'VARSTR', 'VARPSU'],
          dtype='object')




```python
# Update the Headers for Syntax Consistency
cond22 = cond22.rename(columns={'DUPERSID':'Participant ID',
                                  'ICD10CDX': 'ICD-10 Code',
                                  'CONDIDX': 'Condition ID',
                                  'IPCOND': 'Inpatient Events',
                                  'OPCOND': 'Outpatient Events',
                                  'OBCOND': 'Office-Based Events',
                                  'HHCOND':'Home Health Events',
                                  'PERWT22F': 'Expenditure Participant Weight'})

# View the new columns and update the variable
# Pass the columns to the variable:
cond22Columns = cond22.columns
# Call the variable to see the contents
cond22Columns
```




    Index(['DUID', 'PID', 'Participant ID', 'CONDN', 'Condition ID', 'PANEL',
           'CONDRN', 'AGEDIAG', 'CRND1', 'CRND2', 'CRND3', 'CRND4', 'CRND5',
           'CRND6', 'CRND7', 'CRND8', 'CRND9', 'INJURY', 'ACCDNWRK', 'ICD-10 Code',
           'CCSR1X', 'CCSR2X', 'CCSR3X', 'CCSR4X', 'Home Health Events',
           'Inpatient Events', 'Outpatient Events', 'Office-Based Events',
           'ERCOND', 'RXCOND', 'Expenditure Participant Weight', 'VARSTR',
           'VARPSU'],
          dtype='object')




```python
# Missing Values

# Determine the number of missing values
cond22.isnull().sum()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>DUID</th>
      <td>0</td>
    </tr>
    <tr>
      <th>PID</th>
      <td>0</td>
    </tr>
    <tr>
      <th>Participant ID</th>
      <td>0</td>
    </tr>
    <tr>
      <th>CONDN</th>
      <td>0</td>
    </tr>
    <tr>
      <th>Condition ID</th>
      <td>0</td>
    </tr>
    <tr>
      <th>PANEL</th>
      <td>0</td>
    </tr>
    <tr>
      <th>CONDRN</th>
      <td>0</td>
    </tr>
    <tr>
      <th>AGEDIAG</th>
      <td>0</td>
    </tr>
    <tr>
      <th>CRND1</th>
      <td>0</td>
    </tr>
    <tr>
      <th>CRND2</th>
      <td>0</td>
    </tr>
    <tr>
      <th>CRND3</th>
      <td>0</td>
    </tr>
    <tr>
      <th>CRND4</th>
      <td>0</td>
    </tr>
    <tr>
      <th>CRND5</th>
      <td>0</td>
    </tr>
    <tr>
      <th>CRND6</th>
      <td>0</td>
    </tr>
    <tr>
      <th>CRND7</th>
      <td>0</td>
    </tr>
    <tr>
      <th>CRND8</th>
      <td>0</td>
    </tr>
    <tr>
      <th>CRND9</th>
      <td>0</td>
    </tr>
    <tr>
      <th>INJURY</th>
      <td>0</td>
    </tr>
    <tr>
      <th>ACCDNWRK</th>
      <td>0</td>
    </tr>
    <tr>
      <th>ICD-10 Code</th>
      <td>0</td>
    </tr>
    <tr>
      <th>CCSR1X</th>
      <td>0</td>
    </tr>
    <tr>
      <th>CCSR2X</th>
      <td>0</td>
    </tr>
    <tr>
      <th>CCSR3X</th>
      <td>0</td>
    </tr>
    <tr>
      <th>CCSR4X</th>
      <td>0</td>
    </tr>
    <tr>
      <th>Home Health Events</th>
      <td>0</td>
    </tr>
    <tr>
      <th>Inpatient Events</th>
      <td>0</td>
    </tr>
    <tr>
      <th>Outpatient Events</th>
      <td>0</td>
    </tr>
    <tr>
      <th>Office-Based Events</th>
      <td>0</td>
    </tr>
    <tr>
      <th>ERCOND</th>
      <td>0</td>
    </tr>
    <tr>
      <th>RXCOND</th>
      <td>0</td>
    </tr>
    <tr>
      <th>Expenditure Participant Weight</th>
      <td>0</td>
    </tr>
    <tr>
      <th>VARSTR</th>
      <td>0</td>
    </tr>
    <tr>
      <th>VARPSU</th>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div><br><label><b>dtype:</b> int64</label>




```python
# create a function to determine the percentage of missing values

def missing(DataFrame):
    print ('Percentage of missing values in the dataset:\n',
           round((DataFrame.isnull().sum() * 100/ len(DataFrame)),2).sort_values(ascending=False))


# Call the function and execute
missing(cond22)
```

    Percentage of missing values in the dataset:
     DUID                             0
    PID                              0
    Participant ID                   0
    CONDN                            0
    Condition ID                     0
    PANEL                            0
    CONDRN                           0
    AGEDIAG                          0
    CRND1                            0
    CRND2                            0
    CRND3                            0
    CRND4                            0
    CRND5                            0
    CRND6                            0
    CRND7                            0
    CRND8                            0
    CRND9                            0
    INJURY                           0
    ACCDNWRK                         0
    ICD-10 Code                      0
    CCSR1X                           0
    CCSR2X                           0
    CCSR3X                           0
    CCSR4X                           0
    Home Health Events               0
    Inpatient Events                 0
    Outpatient Events                0
    Office-Based Events              0
    ERCOND                           0
    RXCOND                           0
    Expenditure Participant Weight   0
    VARSTR                           0
    VARPSU                           0
    dtype: float64


There are no missing values in the Medical Conditions dataset.


```python
# Make a copy of the DataFrame before manipulation
cond22OG = cond22
```


```python
# Dropping columns from the dataframe
cond22 = cond22.drop(['DUID', 'PID','CONDN', 'PANEL', 'CONDRN','AGEDIAG' , 'CRND1', 'CRND2', 'CRND3', 'CRND4', 'CRND5', 'CRND6', 'CRND7', 'CRND8', 'CRND9', 'INJURY', 'ACCDNWRK','CCSR1X', 'CCSR2X', 'CCSR3X', 'CCSR4X','ERCOND', 'RXCOND','VARSTR', 'VARPSU' ],
            axis=1)

# Check the .info() for results
cond22.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 83173 entries, 0 to 83172
    Data columns (total 8 columns):
     #   Column                          Non-Null Count  Dtype  
    ---  ------                          --------------  -----  
     0   Participant ID                  83173 non-null  int64  
     1   Condition ID                    83173 non-null  int64  
     2   ICD-10 Code                     83173 non-null  object 
     3   Home Health Events              83173 non-null  int64  
     4   Inpatient Events                83173 non-null  int64  
     5   Outpatient Events               83173 non-null  int64  
     6   Office-Based Events             83173 non-null  int64  
     7   Expenditure Participant Weight  83173 non-null  float64
    dtypes: float64(1), int64(6), object(1)
    memory usage: 5.1+ MB



```python
# prompt: import a .csv file from github and print the data frame

import pandas as pd

# Replace 'YOUR_GITHUB_RAW_URL' with the actual raw URL of your CSV file on GitHub
# To get the raw URL, click on the "Raw" button on the GitHub page displaying the file.
url = 'https://raw.githubusercontent.com/nailah21400/nailah21400/main/MEPS_2022_data_cleaned.csv'

df = pd.read_csv(url)
print(df)
```

           DUPERSID ICD10CDX        CONDIDX  IPCOND  OPCOND  OBCOND  HHCOND  \
    0    2460398101      F84  2460398101017       2       2       1       2   
    1    2460945102      F84  2460945102005       2       1       1       2   
    2    2461516106      F84  2461516106004       2       2       2       1   
    3    2461516107      F84  2461516107005       2       2       2       1   
    4    2462020102      F84  2462020102004       2       2       2       2   
    5    2462649101      R47  2462649101007       2       2       2       2   
    6    2462845102      F84  2462845102007       2       2       1       2   
    7    2462975101      F84  2462975101004       2       1       1       2   
    8    2463437101      F84  2463437101010       2       2       1       1   
    9    2463830103      F84  2463830103005       2       2       1       1   
    10   2463903104      F80  2463903104003       2       2       2       1   
    11   2463903104      F84  2463903104004       2       2       1       1   
    12   2464442104      R47  2464442104001       2       2       2       1   
    13   2464442104      F84  2464442104003       2       1       1       1   
    14   2464590104      F84  2464590104003       2       2       1       2   
    15   2464847106      F84  2464847106009       2       2       1       2   
    16   2465247103      F84  2465247103005       2       2       1       2   
    17   2465282103      F80  2465282103008       2       2       1       2   
    18   2465282103      F84  2465282103009       2       2       1       2   
    19   2465455101      F84  2465455101001       2       2       1       2   
    20   2465683102      F84  2465683102001       2       2       2       1   
    21   2466055104      F84  2466055104011       2       1       1       1   
    22   2466341102      F84  2466341102005       2       1       2       2   
    23   2466720106      R47  2466720106011       2       2       1       2   
    24   2466720109      F80  2466720109001       2       2       1       2   
    25   2467659104      F80  2467659104001       2       2       1       2   
    26   2467911102      F84  2467911102017       2       2       1       1   
    27   2468292101      F84  2468292101008       2       2       1       2   
    28   2468292103      F84  2468292103004       2       2       1       2   
    29   2468965103      F80  2468965103010       2       2       1       2   
    30   2469281101      R47  2469281101029       2       2       1       2   
    31   2469355103      F84  2469355103001       2       2       1       2   
    32   2469408201      F80  2469408201004       2       2       1       1   
    33   2469408201      R47  2469408201005       2       2       1       1   
    34   2469478102      F84  2469478102001       2       2       2       1   
    35   2469672102      F84  2469672102009       2       2       1       2   
    36   2469672104      F84  2469672104008       2       1       1       2   
    37   2469672105      F84  2469672105006       2       2       1       2   
    38   2680161103      F84  2680161103005       2       2       1       2   
    39   2680330104      F84  2680330104002       2       1       1       2   
    40   2680512103      F80  2680512103001       2       2       2       1   
    41   2680548103      F84  2680548103001       2       2       2       1   
    42   2680570103      F84  2680570103006       2       2       1       2   
    43   2680632104      F80  2680632104003       2       2       2       1   
    44   2680632104      R47  2680632104004       2       2       2       1   
    45   2680723302      F80  2680723302010       2       2       1       2   
    46   2680764103      F80  2680764103001       2       2       1       2   
    47   2680938102      F84  2680938102001       2       2       1       2   
    48   2681082105      F80  2681082105003       2       2       1       1   
    49   2681190103      F80  2681190103001       2       2       2       2   
    50   2681667103      F84  2681667103002       2       2       1       2   
    51   2681799105      F80  2681799105003       2       2       1       2   
    52   2681900102      R47  2681900102019       2       2       1       1   
    53   2681922103      F84  2681922103002       2       2       1       2   
    54   2682286104      F80  2682286104002       2       2       1       2   
    55   2682313101      F84  2682313101007       2       2       1       2   
    56   2682431102      F84  2682431102001       2       2       1       2   
    57   2683031101      R47  2683031101008       2       2       2       2   
    58   2683067103      F80  2683067103001       2       2       2       2   
    59   2683165102      F84  2683165102002       2       2       1       2   
    60   2683465104      F84  2683465104001       2       2       1       2   
    61   2683517105      F84  2683517105001       2       2       1       1   
    62   2683770103      F84  2683770103003       2       2       1       2   
    63   2683886103      F84  2683886103002       2       2       1       1   
    64   2683886103      F84  2683886103011       2       2       1       2   
    65   2684020104      F84  2684020104001       2       2       1       2   
    66   2684334105      R47  2684334105002       2       2       2       1   
    67   2684334106      R47  2684334106001       2       2       2       1   
    68   2684435104      R47  2684435104001       2       2       1       2   
    69   2684459103      F84  2684459103006       2       1       1       2   
    70   2684504104      F80  2684504104003       2       2       1       2   
    71   2684658104      F80  2684658104002       2       2       1       2   
    72   2684793104      R47  2684793104003       2       2       1       2   
    73   2684813103      F80  2684813103007       2       2       2       1   
    74   2684923105      F84  2684923105002       2       2       1       2   
    75   2685057103      R47  2685057103006       2       2       1       2   
    76   2685293104      R47  2685293104002       2       2       1       2   
    77   2685420105      F80  2685420105006       2       2       1       2   
    78   2685420105      F84  2685420105008       2       2       1       2   
    79   2685520104      R47  2685520104001       2       2       1       2   
    80   2685672103      R47  2685672103005       2       2       1       1   
    81   2685737103      R47  2685737103002       2       2       1       2   
    82   2685851103      F84  2685851103001       2       2       1       1   
    83   2685959103      F84  2685959103002       2       1       1       2   
    84   2686050104      R47  2686050104002       2       2       1       2   
    85   2686365104      F84  2686365104001       2       2       1       1   
    86   2686576105      R47  2686576105004       2       2       1       2   
    87   2686906102      F84  2686906102004       2       2       1       1   
    88   2687233104      F84  2687233104006       2       1       1       2   
    89   2687663103      F84  2687663103003       2       2       1       2   
    90   2687766103      R47  2687766103001       2       2       1       2   
    91   2687775103      F84  2687775103001       2       2       1       2   
    92   2688018104      F84  2688018104006       2       1       1       2   
    93   2688142104      F84  2688142104002       2       2       2       2   
    94   2689259104      F80  2689259104006       2       2       1       2   
    95   2689287103      F84  2689287103004       2       2       1       2   
    96   2689287104      F84  2689287104001       2       2       1       2   
    97   2790019101      F84  2790019101004       1       1       2       2   
    98   2790071103      R47  2790071103004       2       2       1       2   
    99   2790071103      F84  2790071103009       2       1       2       2   
    100  2790071104      R47  2790071104002       2       2       1       2   
    101  2790071104      F84  2790071104013       2       1       2       2   
    102  2790071105      F80  2790071105008       2       2       1       2   
    103  2790133104      F84  2790133104006       2       2       1       2   
    104  2790197104      F84  2790197104009       2       2       2       2   
    105  2790197105      F84  2790197105001       2       1       1       2   
    106  2790331104      F80  2790331104002       2       2       1       2   
    107  2790501104      F84  2790501104002       2       2       1       1   
    108  2790501105      F84  2790501105008       2       2       2       1   
    109  2790513103      F80  2790513103002       2       2       2       1   
    110  2790680103      R47  2790680103001       2       1       2       2   
    111  2790865104      F84  2790865104002       2       2       1       2   
    112  2790944103      R47  2790944103003       2       2       1       2   
    113  2791090104      F84  2791090104002       2       2       1       2   
    114  2791232102      F84  2791232102003       2       2       1       1   
    115  2791537103      F84  2791537103002       2       2       1       2   
    116  2791557103      R47  2791557103001       2       2       1       2   
    117  2791801101      R47  2791801101007       2       2       2       2   
    118  2791969103      F84  2791969103003       2       2       2       2   
    119  2791998103      F84  2791998103001       2       2       1       2   
    120  2792167106      F84  2792167106001       2       2       1       2   
    121  2792172103      F80  2792172103008       2       2       1       2   
    122  2792316103      F84  2792316103001       2       2       1       2   
    123  2792316103      F84  2792316103002       2       2       1       2   
    124  2792406102      F84  2792406102002       2       2       1       2   
    125  2792406103      F80  2792406103002       2       2       1       2   
    126  2792452103      F80  2792452103001       2       2       2       1   
    127  2792672102      R47  2792672102002       2       2       1       2   
    128  2792734104      F84  2792734104002       2       2       1       2   
    129  2793048102      F84  2793048102001       2       2       1       2   
    130  2793125106      F80  2793125106002       2       2       1       2   
    131  2793223106      R47  2793223106001       2       2       1       2   
    132  2793277105      F80  2793277105001       2       1       2       2   
    133  2793306102      F84  2793306102006       2       2       1       2   
    134  2793568105      R47  2793568105001       2       2       1       2   
    135  2793836104      R47  2793836104002       2       2       1       2   
    136  2793836104      F80  2793836104004       2       2       2       1   
    137  2794036103      F84  2794036103003       2       1       1       2   
    138  2794044102      F84  2794044102003       2       2       1       2   
    139  2794430102      R47  2794430102013       2       2       2       2   
    140  2794519104      R47  2794519104001       2       2       1       2   
    141  2794715103      F84  2794715103002       2       2       1       2   
    142  2794747102      F84  2794747102004       2       2       2       1   
    143  2794754104      F84  2794754104001       2       2       1       2   
    144  2794758105      F84  2794758105001       2       2       1       2   
    145  2794934113      F80  2794934113002       2       1       2       2   
    146  2794934114      F80  2794934114001       2       1       2       2   
    147  2795047104      F80  2795047104001       2       2       1       2   
    148  2795047104      F84  2795047104003       2       1       2       2   
    149  2795092103      F84  2795092103002       2       2       1       1   
    150  2795117105      F84  2795117105002       2       2       1       2   
    151  2795154103      F84  2795154103004       2       2       1       2   
    152  2795162104      R47  2795162104005       2       2       2       1   
    153  2795162104      F84  2795162104019       2       2       2       1   
    154  2795536105      F84  2795536105005       2       2       1       2   
    155  2795570101      F84  2795570101001       2       2       1       2   
    156  2795640102      F80  2795640102003       2       2       1       2   
    157  2795640104      F80  2795640104003       2       2       1       2   
    158  2795647103      F80  2795647103001       2       2       1       2   
    159  2795701106      F80  2795701106003       2       2       1       1   
    160  2795788103      F80  2795788103001       2       2       1       2   
    161  2796449104      F80  2796449104001       2       2       2       1   
    162  2796518104      F84  2796518104002       2       2       1       2   
    163  2796573104      F84  2796573104001       2       2       1       2   
    164  2796859105      R47  2796859105001       2       2       1       2   
    165  2797008104      R47  2797008104002       2       2       1       2   
    166  2797008106      R47  2797008106002       2       2       1       2   
    167  2797144104      R47  2797144104004       2       2       1       2   
    168  2797188104      R47  2797188104001       2       2       1       2   
    169  2797188105      R47  2797188105001       2       2       1       2   
    170  2797188106      R47  2797188106003       2       2       1       2   
    171  2797226102      F84  2797226102001       2       2       1       2   
    172  2797539103      R47  2797539103001       2       2       1       2   
    173  2797765104      R47  2797765104002       2       2       1       2   
    174  2797798102      F84  2797798102004       2       2       1       2   
    175  2797874103      F84  2797874103003       2       2       1       2   
    176  2797874105      F80  2797874105006       2       2       1       2   
    177  2797908103      F80  2797908103001       2       2       1       2   
    178  2797908104      R47  2797908104001       2       2       1       2   
    179  2798303103      F84  2798303103002       2       2       1       2   
    180  2798327101      R47  2798327101003       1       2       2       2   
    181  2798428104      F80  2798428104001       2       1       1       2   
    182  2798498103      F80  2798498103001       2       2       1       2   
    183  2798587101      F80  2798587101021       2       2       2       2   
    184  2798845103      F80  2798845103003       2       2       1       1   
    185  2798926102      F80  2798926102003       2       2       1       2   
    186  2798961103      F84  2798961103003       2       2       2       2   
    187  2799058103      F84  2799058103001       2       2       1       2   
    188  2799072105      F80  2799072105005       2       2       1       2   
    189  2799132103      F84  2799132103004       2       2       1       1   
    190  2799152102      F84  2799152102005       2       2       1       2   
    191  2799192103      F84  2799192103001       2       2       2       1   
    192  2799284104      F84  2799284104003       2       2       1       1   
    193  2799305102      F84  2799305102002       2       2       1       2   
    194  2799383102      F80  2799383102001       2       2       1       2   
    195  2799484103      F84  2799484103005       2       2       2       2   
    196  2799508103      F84  2799508103001       2       2       2       1   
    197  2799596104      R47  2799596104002       2       2       1       2   
    
         PERWT22F  INSURC22  RACEV1X  RACEBX  MCREV22  CHTHER42  CHSRHB42  \
    0       10078         2        1       3        1        -1        -1   
    1       14065         1        1       3        2         2        -1   
    2        4481         2        1       3        2         1         1   
    3        4481         2        1       3        2         1         1   
    4        5499         2        1       3        2        -1        -1   
    5       21883         1        1       3        2        -1        -1   
    6       13931         2        1       3        2        -1        -1   
    7        8131         2        1       3        1        -1        -1   
    8        2796         1        6       3        2        -1        -1   
    9        5047         1        1       3        1        -1        -1   
    10       4048         1        2       1        2         1         1   
    11       4048         1        2       1        2         1         1   
    12      10644         2        1       3        2         1         1   
    13      10644         2        1       3        2         1         1   
    14      22647         1        4       3        2         2         1   
    15      21540         1        1       3        2         1         1   
    16          0         1        1       3        2         2         1   
    17      18153         2        1       3        2         1         1   
    18      18153         2        1       3        2         1         1   
    19      11609         2        1       3        1        -1        -1   
    20      29182         2        6       3        2        -1        -1   
    21      11599         2        1       3        2         1         1   
    22       4941         1        6       3        2        -1        -1   
    23      10421         2        1       3        2         2        -1   
    24          0         2        1       3        2         2        -1   
    25       6237         2        2       1        2         1        -1   
    26      16450         2        1       3        2        -1        -1   
    27      17538         1        1       3        2        -1        -1   
    28      17806         1        1       3        2         2         1   
    29      40057         2        1       3        2         1         1   
    30      11037         5        1       3        1        -1        -1   
    31      42542         1        1       3        2         1         1   
    32       8121         1        1       3        2         1         1   
    33       8121         1        1       3        2         1         1   
    34       3247         2        2       1        2        -1        -1   
    35       6952         1        1       3        1        -1        -1   
    36      10372         1        1       3        2         1        -1   
    37       9683         1        1       3        2         1         1   
    38      18485         2        1       3        2         2         1   
    39      10044         1        1       3        2         1         1   
    40      24979         1        1       3        2         1        -1   
    41      30893         1        1       3        2        -1        -1   
    42       5576         2        1       3        2         2         1   
    43       6078         1        1       3        2         1        -1   
    44       6078         1        1       3        2         1        -1   
    45      27220         2        1       3        2         2        -1   
    46       1596         1        4       3        2         1        -1   
    47       7748         1        1       3        2         2         1   
    48      18948         1        1       3        2         1        -1   
    49       6202         2        1       3        2         1         1   
    50      18123         1        1       3        2         2        -1   
    51      10943         1        1       3        2         1        -1   
    52       7479         4        4       3        1        -1        -1   
    53      26975         2        1       3        2         2        -1   
    54       5591         1        6       3        2         1        -1   
    55      19281         1        1       3        2        -1        -1   
    56      10105         1        1       3        2        -1        -1   
    57       8583         5        1       3        1        -1        -1   
    58       4529         2        4       3        2         1         1   
    59      19389         3        1       3        2         1         1   
    60      17939         1        1       3        2         1         1   
    61      15362         1        1       3        2         1         1   
    62      14952         1        1       3        2        -1        -1   
    63      10703         2        1       3        2         1        -1   
    64      10703         2        1       3        2         1        -1   
    65       8962         2        1       3        2         1         1   
    66      25158         2        1       3        2         1        -1   
    67      25158         2        1       3        2         1        -1   
    68      13748         2        1       3        2         1        -1   
    69       1733         1        6       2        2         2         1   
    70       2230         1        6       3        2         2         1   
    71       4476         1        1       3        2         1        -1   
    72      18979         1        1       3        2         2        -1   
    73      23773         1        6       3        2         1         2   
    74      23824         2        6       2        2         1         2   
    75       5826         1        1       3        2         2        -1   
    76      23977         2        1       3        2         2        -1   
    77      28705         2        1       3        2         1         1   
    78      28705         2        1       3        2         1         1   
    79      11073         1        1       3        2         1         2   
    80       6697         2        1       3        2        -1        -1   
    81      14180         2        1       3        2         1         1   
    82      17902         1        1       3        2         1         1   
    83      63685         1        1       3        2        -1        -1   
    84      17855         2        2       1        2         1        -1   
    85      26139         1        1       3        2         2         1   
    86       6268         1        1       3        2         1         1   
    87      16989         2        1       3        2        -1        -1   
    88      27183         2        1       3        2         1         1   
    89      13180         2        1       3        2         1         1   
    90      13837         2        1       3        2         1        -1   
    91      20185         2        1       3        2        -1        -1   
    92      12895         1        1       3        2        -1        -1   
    93      28724         2        1       3        2         1         1   
    94      17462         1        1       3        2         1         1   
    95       8751         1        1       3        2         2         1   
    96       7543         1        1       3        2         1         1   
    97       9937         2        1       3        1        -1        -1   
    98      32773         1        1       3        2         1         1   
    99      32773         1        1       3        2         1         1   
    100     41304         1        1       3        2         1         1   
    101     41304         1        1       3        2         1         1   
    102     34207         1        1       3        2         1         1   
    103     14737         1        1       3        2         2         1   
    104     12593         1        1       3        2         1         1   
    105     12813         1        1       3        2         1         1   
    106     14984         1        1       3        2         1         1   
    107      6203         2        1       3        2         1         1   
    108      6388         2        1       3        2         1         1   
    109     23143         2        1       3        2         2        -1   
    110      8538         2        2       1        2         2        -1   
    111     16780         1        1       3        2         2         1   
    112      4946         1        1       3        2         1        -1   
    113     86592         1        1       3        2        -1        -1   
    114      1028         2        6       2        2         1         1   
    115     31846         1        1       3        2         2         1   
    116      6422         2        1       3        2         1        -1   
    117     11312         6        1       3        1        -1        -1   
    118     22569         1        1       3        2        -1        -1   
    119     19786         1        1       3        2         1         1   
    120     11323         2        1       3        2         1         1   
    121     33420         1        2       1        2         1        -8   
    122     29063         2        2       1        2        -1        -1   
    123     29063         2        2       1        2        -1        -1   
    124      4845         1        1       3        2         2         1   
    125      5396         1        1       3        2         1        -1   
    126     10154         1        4       3        2         2        -1   
    127      7240         2        2       1        2         1        -1   
    128     10487         1        2       1        2         1         1   
    129     10195         1        1       3        2         1         1   
    130     12879         1        1       3        2         2        -1   
    131     21317         1        1       3        2         1         1   
    132     23216         1        1       3        2         1         1   
    133     23141         2        1       3        2         1         1   
    134     11578         1        1       3        2         2        -1   
    135     42061         2        1       3        2         1         1   
    136     42061         2        1       3        2         1         1   
    137     53802         1        1       3        2         2        -1   
    138     13114         2        2       1        1        -1        -1   
    139     19081         4        1       3        1        -1        -1   
    140      8731         1        1       3        2         1        -1   
    141      7776         2        1       3        2         1        -1   
    142     13429         1        1       3        1         1         1   
    143      8356         2        1       3        2         1         1   
    144     21788         1        1       3        2         1         1   
    145     11720         2        1       3        2        -8        -1   
    146     17172         2        1       3        2        -8        -1   
    147     10878         2        1       3        2         1         1   
    148     10878         2        1       3        2         1         1   
    149     15852         1        1       3        2         1         1   
    150     13563         1        1       3        2         1         1   
    151     55895         1        1       3        2         2         1   
    152     11787         2        1       3        2         1         1   
    153     11787         2        1       3        2         1         1   
    154      9221         1        1       3        2         2         1   
    155     29711         1        1       3        2        -1        -1   
    156     54520         2        1       3        2         1         1   
    157     49746         2        1       3        2         1        -1   
    158      9174         1        4       3        2         1         1   
    159     18734         1        1       3        2         1        -1   
    160     22536         1        1       3        2         2        -1   
    161      3134         2        1       3        2         1        -1   
    162     19991         2        2       1        2         2        -1   
    163     10271         1        6       2        2         1         1   
    164     30083         2        1       3        2         1        -1   
    165     30386         1        1       3        2         1        -1   
    166     34345         1        1       3        2         1        -1   
    167     17896         1        1       3        2         1        -1   
    168     12970         2        1       3        2         1        -1   
    169     12970         2        1       3        2         1        -1   
    170     13970         2        1       3        2         1        -1   
    171     17585         2        2       1        2         1         1   
    172      9273         2        2       1        2         1        -1   
    173     15289         1        1       3        2         1         1   
    174     17299         2        1       3        2        -1        -1   
    175     15039         1        1       3        2         1         1   
    176      8699         1        1       3        2         1         1   
    177     33946         1        1       3        2         1        -1   
    178     34955         1        1       3        2         1        -1   
    179     17738         2        1       3        2         2        -1   
    180     11627         5        1       3        1        -1        -1   
    181     21104         1        1       3        2        -7        -1   
    182     37233         2        1       3        2         1        -1   
    183     13693         5        1       3        1        -1        -1   
    184     30293         1        2       1        2         1        -1   
    185      7052         2        1       3        2         1         1   
    186     21091         1        6       2        2        -1        -1   
    187     21086         1        1       3        2         1        -1   
    188     14389         2        1       3        2        -1        -1   
    189     14936         2        1       3        2         1         1   
    190      5766         2        1       3        2        -1        -1   
    191     18344         1        2       1        2         1         1   
    192     14654         1        1       3        2         1         1   
    193     15536         2        1       3        2        -1        -1   
    194     10552         2        1       3        2         1         1   
    195     40934         1        6       3        2         2         1   
    196     12890         2        1       3        2        -1        -1   
    197     67064         1        1       3        2         1         1   
    
         HWELLSPK  RTHLTH53  
    0          -1         2  
    1           1         1  
    2          -1         4  
    3          -1         5  
    4          -1         5  
    5           1         3  
    6          -1         2  
    7          -1         3  
    8          -1         3  
    9          -1         3  
    10          5         2  
    11          5         2  
    12         -1         3  
    13         -1         3  
    14          5         4  
    15         -1         4  
    16         -1         2  
    17          5         4  
    18          5         4  
    19         -1         4  
    20         -1         4  
    21          5         1  
    22         -1         5  
    23          5         3  
    24          5         1  
    25          5         1  
    26         -1         2  
    27         -1         2  
    28          5         2  
    29          5         2  
    30         -1         3  
    31          5         3  
    32          5         2  
    33          5         2  
    34         -1         3  
    35         -1         3  
    36         -1         3  
    37          5         3  
    38         -1         2  
    39         -1         2  
    40          5         1  
    41         -1         2  
    42         -1         1  
    43          5         2  
    44          5         2  
    45          5         3  
    46          5         3  
    47         -1         3  
    48          5         1  
    49          5         3  
    50         -1         2  
    51          5         2  
    52         -1         4  
    53          1         3  
    54         -1         2  
    55         -1         5  
    56         -1         2  
    57         -1        -1  
    58          5         4  
    59          5         1  
    60          5         1  
    61         -1         1  
    62         -1         1  
    63         -1         2  
    64         -1         2  
    65          2         3  
    66          5         3  
    67          5         3  
    68         -1         3  
    69          1         4  
    70         -1         1  
    71          5         1  
    72          1         1  
    73          5         1  
    74          5         4  
    75         -1         1  
    76          5         2  
    77          5         4  
    78          5         4  
    79          5         1  
    80          1         2  
    81         -1         3  
    82         -1         2  
    83         -1         1  
    84         -1         3  
    85         -1         2  
    86         -1         2  
    87         -1         3  
    88         -1         3  
    89         -1         2  
    90          1         1  
    91         -1         2  
    92         -1         1  
    93         -1         1  
    94          5         1  
    95         -1         1  
    96         -1         3  
    97         -1         3  
    98         -1         3  
    99         -1         3  
    100         5         3  
    101         5         3  
    102         5         3  
    103        -1         3  
    104        -1         4  
    105        -1         2  
    106        -1         2  
    107        -1         1  
    108         5         1  
    109         5         1  
    110         5         1  
    111        -1         1  
    112        -1         1  
    113        -1         1  
    114        -1         3  
    115        -1         3  
    116         5         3  
    117        -1         4  
    118        -1         2  
    119         5         4  
    120         5         3  
    121         5         2  
    122        -1         1  
    123        -1         1  
    124        -1         3  
    125        -1         1  
    126         5         1  
    127         5         1  
    128        -1         3  
    129        -1         1  
    130         5         1  
    131         5         3  
    132         5         3  
    133         5         1  
    134        -1         1  
    135         5         3  
    136         5         3  
    137         5         1  
    138        -1         4  
    139         2         4  
    140        -1         2  
    141        -1         3  
    142        -1         5  
    143        -1         1  
    144         1         3  
    145         1         4  
    146         1         4  
    147         5         1  
    148         5         1  
    149        -1         2  
    150        -1         4  
    151        -1         4  
    152        -1         4  
    153        -1         4  
    154        -1         3  
    155        -1         2  
    156        -1         2  
    157        -1         3  
    158         5         2  
    159         5         2  
    160        -1         2  
    161         5         1  
    162         5         2  
    163        -1         1  
    164         5         1  
    165        -1         1  
    166         5         1  
    167         5         1  
    168         5         2  
    169         5         2  
    170         5         4  
    171         5         3  
    172        -1         2  
    173        -1         1  
    174        -1         4  
    175         1         2  
    176        -1         2  
    177        -1         1  
    178         5         1  
    179         1         1  
    180        -1         4  
    181         5         3  
    182         5         1  
    183        -1         3  
    184         1         2  
    185         5         4  
    186        -1         2  
    187        -1         1  
    188        -1         3  
    189         5         2  
    190        -1         4  
    191         4         2  
    192        -1         2  
    193        -1         2  
    194         5         3  
    195        -1         2  
    196        -1         3  
    197        -1         1  



```python
meps_data_cleaned = df
```


```python
meps_data_cleaned.shape
```




    (198, 20)




```python
meps_data_cleaned.rename(columns={'DUPERSID':'Participant ID',
                                  'ICD10CDX': 'ICD-10 Code',
                                  'CONDIDX': 'Condition ID',
                                  'IPCOND': 'Inpatient Events',
                                  'OPCOND': 'Outpatient Events',
                                  'OBCOND': 'Office-Based Events',
                                  'HHCOND':'Home Health Events',
                                  'INSURC22': 'Insurance Type',
                                  'RACEV1X': 'Race',
                                  'RTHLTH53': 'Health Status',
                                  'HWELLSPK': 'English Proficiency',
                                  'PERWT22F': 'Expenditure Participant Weight'})
```





  <div id="df-04bdbe60-d51a-4e5f-a94f-18b995b1e7d7" class="colab-df-container">
    <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Participant ID</th>
      <th>ICD-10 Code</th>
      <th>Condition ID</th>
      <th>Inpatient Events</th>
      <th>Outpatient Events</th>
      <th>Office-Based Events</th>
      <th>Home Health Events</th>
      <th>Expenditure Participant Weight</th>
      <th>Insurance Type</th>
      <th>Race</th>
      <th>RACEBX</th>
      <th>MCREV22</th>
      <th>CHTHER42</th>
      <th>CHSRHB42</th>
      <th>English Proficiency</th>
      <th>Health Status</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2460398101</td>
      <td>F84</td>
      <td>2460398101017</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>10078</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2460945102</td>
      <td>F84</td>
      <td>2460945102005</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>14065</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>-1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2461516106</td>
      <td>F84</td>
      <td>2461516106004</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>4481</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>-1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2461516107</td>
      <td>F84</td>
      <td>2461516107005</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>4481</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>-1</td>
      <td>5</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2462020102</td>
      <td>F84</td>
      <td>2462020102004</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>5499</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>5</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2462649101</td>
      <td>R47</td>
      <td>2462649101007</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>21883</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>-1</td>
      <td>-1</td>
      <td>1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2462845102</td>
      <td>F84</td>
      <td>2462845102007</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>13931</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2462975101</td>
      <td>F84</td>
      <td>2462975101004</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>8131</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2463437101</td>
      <td>F84</td>
      <td>2463437101010</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>2796</td>
      <td>1</td>
      <td>6</td>
      <td>3</td>
      <td>2</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2463830103</td>
      <td>F84</td>
      <td>2463830103005</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5047</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>10</th>
      <td>2463903104</td>
      <td>F80</td>
      <td>2463903104003</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>4048</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>2</td>
    </tr>
    <tr>
      <th>11</th>
      <td>2463903104</td>
      <td>F84</td>
      <td>2463903104004</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>4048</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>2</td>
    </tr>
    <tr>
      <th>12</th>
      <td>2464442104</td>
      <td>R47</td>
      <td>2464442104001</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>10644</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>-1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>13</th>
      <td>2464442104</td>
      <td>F84</td>
      <td>2464442104003</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>10644</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>-1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>14</th>
      <td>2464590104</td>
      <td>F84</td>
      <td>2464590104003</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>22647</td>
      <td>1</td>
      <td>4</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>5</td>
      <td>4</td>
    </tr>
    <tr>
      <th>15</th>
      <td>2464847106</td>
      <td>F84</td>
      <td>2464847106009</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>21540</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>-1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>16</th>
      <td>2465247103</td>
      <td>F84</td>
      <td>2465247103005</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>17</th>
      <td>2465282103</td>
      <td>F80</td>
      <td>2465282103008</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>18153</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>4</td>
    </tr>
    <tr>
      <th>18</th>
      <td>2465282103</td>
      <td>F84</td>
      <td>2465282103009</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>18153</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>4</td>
    </tr>
    <tr>
      <th>19</th>
      <td>2465455101</td>
      <td>F84</td>
      <td>2465455101001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>11609</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>20</th>
      <td>2465683102</td>
      <td>F84</td>
      <td>2465683102001</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>29182</td>
      <td>2</td>
      <td>6</td>
      <td>3</td>
      <td>2</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>21</th>
      <td>2466055104</td>
      <td>F84</td>
      <td>2466055104011</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>11599</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>1</td>
    </tr>
    <tr>
      <th>22</th>
      <td>2466341102</td>
      <td>F84</td>
      <td>2466341102005</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>4941</td>
      <td>1</td>
      <td>6</td>
      <td>3</td>
      <td>2</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>5</td>
    </tr>
    <tr>
      <th>23</th>
      <td>2466720106</td>
      <td>R47</td>
      <td>2466720106011</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>10421</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>-1</td>
      <td>5</td>
      <td>3</td>
    </tr>
    <tr>
      <th>24</th>
      <td>2466720109</td>
      <td>F80</td>
      <td>2466720109001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>-1</td>
      <td>5</td>
      <td>1</td>
    </tr>
    <tr>
      <th>25</th>
      <td>2467659104</td>
      <td>F80</td>
      <td>2467659104001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>6237</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>5</td>
      <td>1</td>
    </tr>
    <tr>
      <th>26</th>
      <td>2467911102</td>
      <td>F84</td>
      <td>2467911102017</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>16450</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>27</th>
      <td>2468292101</td>
      <td>F84</td>
      <td>2468292101008</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>17538</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>28</th>
      <td>2468292103</td>
      <td>F84</td>
      <td>2468292103004</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>17806</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>5</td>
      <td>2</td>
    </tr>
    <tr>
      <th>29</th>
      <td>2468965103</td>
      <td>F80</td>
      <td>2468965103010</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>40057</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>2</td>
    </tr>
    <tr>
      <th>30</th>
      <td>2469281101</td>
      <td>R47</td>
      <td>2469281101029</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>11037</td>
      <td>5</td>
      <td>1</td>
      <td>3</td>
      <td>1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>31</th>
      <td>2469355103</td>
      <td>F84</td>
      <td>2469355103001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>42542</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>3</td>
    </tr>
    <tr>
      <th>32</th>
      <td>2469408201</td>
      <td>F80</td>
      <td>2469408201004</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>8121</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>2</td>
    </tr>
    <tr>
      <th>33</th>
      <td>2469408201</td>
      <td>R47</td>
      <td>2469408201005</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>8121</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>2</td>
    </tr>
    <tr>
      <th>34</th>
      <td>2469478102</td>
      <td>F84</td>
      <td>2469478102001</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>3247</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>35</th>
      <td>2469672102</td>
      <td>F84</td>
      <td>2469672102009</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>6952</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>36</th>
      <td>2469672104</td>
      <td>F84</td>
      <td>2469672104008</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>10372</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>-1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>37</th>
      <td>2469672105</td>
      <td>F84</td>
      <td>2469672105006</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>9683</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>3</td>
    </tr>
    <tr>
      <th>38</th>
      <td>2680161103</td>
      <td>F84</td>
      <td>2680161103005</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>18485</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>39</th>
      <td>2680330104</td>
      <td>F84</td>
      <td>2680330104002</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>10044</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>-1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>40</th>
      <td>2680512103</td>
      <td>F80</td>
      <td>2680512103001</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>24979</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>5</td>
      <td>1</td>
    </tr>
    <tr>
      <th>41</th>
      <td>2680548103</td>
      <td>F84</td>
      <td>2680548103001</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>30893</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>42</th>
      <td>2680570103</td>
      <td>F84</td>
      <td>2680570103006</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>5576</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>43</th>
      <td>2680632104</td>
      <td>F80</td>
      <td>2680632104003</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>6078</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>5</td>
      <td>2</td>
    </tr>
    <tr>
      <th>44</th>
      <td>2680632104</td>
      <td>R47</td>
      <td>2680632104004</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>6078</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>5</td>
      <td>2</td>
    </tr>
    <tr>
      <th>45</th>
      <td>2680723302</td>
      <td>F80</td>
      <td>2680723302010</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>27220</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>-1</td>
      <td>5</td>
      <td>3</td>
    </tr>
    <tr>
      <th>46</th>
      <td>2680764103</td>
      <td>F80</td>
      <td>2680764103001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>1596</td>
      <td>1</td>
      <td>4</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>5</td>
      <td>3</td>
    </tr>
    <tr>
      <th>47</th>
      <td>2680938102</td>
      <td>F84</td>
      <td>2680938102001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>7748</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>48</th>
      <td>2681082105</td>
      <td>F80</td>
      <td>2681082105003</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>18948</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>5</td>
      <td>1</td>
    </tr>
    <tr>
      <th>49</th>
      <td>2681190103</td>
      <td>F80</td>
      <td>2681190103001</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>6202</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>3</td>
    </tr>
    <tr>
      <th>50</th>
      <td>2681667103</td>
      <td>F84</td>
      <td>2681667103002</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>18123</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>-1</td>
      <td>-1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>51</th>
      <td>2681799105</td>
      <td>F80</td>
      <td>2681799105003</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>10943</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>5</td>
      <td>2</td>
    </tr>
    <tr>
      <th>52</th>
      <td>2681900102</td>
      <td>R47</td>
      <td>2681900102019</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>7479</td>
      <td>4</td>
      <td>4</td>
      <td>3</td>
      <td>1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>53</th>
      <td>2681922103</td>
      <td>F84</td>
      <td>2681922103002</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>26975</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>-1</td>
      <td>1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>54</th>
      <td>2682286104</td>
      <td>F80</td>
      <td>2682286104002</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>5591</td>
      <td>1</td>
      <td>6</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>-1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>55</th>
      <td>2682313101</td>
      <td>F84</td>
      <td>2682313101007</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>19281</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>5</td>
    </tr>
    <tr>
      <th>56</th>
      <td>2682431102</td>
      <td>F84</td>
      <td>2682431102001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>10105</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>57</th>
      <td>2683031101</td>
      <td>R47</td>
      <td>2683031101008</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>8583</td>
      <td>5</td>
      <td>1</td>
      <td>3</td>
      <td>1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
    </tr>
    <tr>
      <th>58</th>
      <td>2683067103</td>
      <td>F80</td>
      <td>2683067103001</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>4529</td>
      <td>2</td>
      <td>4</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>4</td>
    </tr>
    <tr>
      <th>59</th>
      <td>2683165102</td>
      <td>F84</td>
      <td>2683165102002</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>19389</td>
      <td>3</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>1</td>
    </tr>
    <tr>
      <th>60</th>
      <td>2683465104</td>
      <td>F84</td>
      <td>2683465104001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>17939</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>1</td>
    </tr>
    <tr>
      <th>61</th>
      <td>2683517105</td>
      <td>F84</td>
      <td>2683517105001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>15362</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>-1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>62</th>
      <td>2683770103</td>
      <td>F84</td>
      <td>2683770103003</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>14952</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>63</th>
      <td>2683886103</td>
      <td>F84</td>
      <td>2683886103002</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>10703</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>-1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>64</th>
      <td>2683886103</td>
      <td>F84</td>
      <td>2683886103011</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>10703</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>-1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>65</th>
      <td>2684020104</td>
      <td>F84</td>
      <td>2684020104001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>8962</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>66</th>
      <td>2684334105</td>
      <td>R47</td>
      <td>2684334105002</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>25158</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>5</td>
      <td>3</td>
    </tr>
    <tr>
      <th>67</th>
      <td>2684334106</td>
      <td>R47</td>
      <td>2684334106001</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>25158</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>5</td>
      <td>3</td>
    </tr>
    <tr>
      <th>68</th>
      <td>2684435104</td>
      <td>R47</td>
      <td>2684435104001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>13748</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>-1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>69</th>
      <td>2684459103</td>
      <td>F84</td>
      <td>2684459103006</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>1733</td>
      <td>1</td>
      <td>6</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>70</th>
      <td>2684504104</td>
      <td>F80</td>
      <td>2684504104003</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>2230</td>
      <td>1</td>
      <td>6</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>71</th>
      <td>2684658104</td>
      <td>F80</td>
      <td>2684658104002</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>4476</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>5</td>
      <td>1</td>
    </tr>
    <tr>
      <th>72</th>
      <td>2684793104</td>
      <td>R47</td>
      <td>2684793104003</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>18979</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>-1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>73</th>
      <td>2684813103</td>
      <td>F80</td>
      <td>2684813103007</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>23773</td>
      <td>1</td>
      <td>6</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>5</td>
      <td>1</td>
    </tr>
    <tr>
      <th>74</th>
      <td>2684923105</td>
      <td>F84</td>
      <td>2684923105002</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>23824</td>
      <td>2</td>
      <td>6</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>5</td>
      <td>4</td>
    </tr>
    <tr>
      <th>75</th>
      <td>2685057103</td>
      <td>R47</td>
      <td>2685057103006</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>5826</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>-1</td>
      <td>-1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>76</th>
      <td>2685293104</td>
      <td>R47</td>
      <td>2685293104002</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>23977</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>-1</td>
      <td>5</td>
      <td>2</td>
    </tr>
    <tr>
      <th>77</th>
      <td>2685420105</td>
      <td>F80</td>
      <td>2685420105006</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>28705</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>4</td>
    </tr>
    <tr>
      <th>78</th>
      <td>2685420105</td>
      <td>F84</td>
      <td>2685420105008</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>28705</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>4</td>
    </tr>
    <tr>
      <th>79</th>
      <td>2685520104</td>
      <td>R47</td>
      <td>2685520104001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>11073</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>5</td>
      <td>1</td>
    </tr>
    <tr>
      <th>80</th>
      <td>2685672103</td>
      <td>R47</td>
      <td>2685672103005</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>6697</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>-1</td>
      <td>-1</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>81</th>
      <td>2685737103</td>
      <td>R47</td>
      <td>2685737103002</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>14180</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>-1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>82</th>
      <td>2685851103</td>
      <td>F84</td>
      <td>2685851103001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>17902</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>-1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>83</th>
      <td>2685959103</td>
      <td>F84</td>
      <td>2685959103002</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>63685</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>84</th>
      <td>2686050104</td>
      <td>R47</td>
      <td>2686050104002</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>17855</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>-1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>85</th>
      <td>2686365104</td>
      <td>F84</td>
      <td>2686365104001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>26139</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>86</th>
      <td>2686576105</td>
      <td>R47</td>
      <td>2686576105004</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>6268</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>-1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>87</th>
      <td>2686906102</td>
      <td>F84</td>
      <td>2686906102004</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>16989</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>88</th>
      <td>2687233104</td>
      <td>F84</td>
      <td>2687233104006</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>27183</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>-1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>89</th>
      <td>2687663103</td>
      <td>F84</td>
      <td>2687663103003</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>13180</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>-1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>90</th>
      <td>2687766103</td>
      <td>R47</td>
      <td>2687766103001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>13837</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>91</th>
      <td>2687775103</td>
      <td>F84</td>
      <td>2687775103001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>20185</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>92</th>
      <td>2688018104</td>
      <td>F84</td>
      <td>2688018104006</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>12895</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>93</th>
      <td>2688142104</td>
      <td>F84</td>
      <td>2688142104002</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>28724</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>-1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>94</th>
      <td>2689259104</td>
      <td>F80</td>
      <td>2689259104006</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>17462</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>1</td>
    </tr>
    <tr>
      <th>95</th>
      <td>2689287103</td>
      <td>F84</td>
      <td>2689287103004</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>8751</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>96</th>
      <td>2689287104</td>
      <td>F84</td>
      <td>2689287104001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>7543</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>-1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>97</th>
      <td>2790019101</td>
      <td>F84</td>
      <td>2790019101004</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>9937</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>98</th>
      <td>2790071103</td>
      <td>R47</td>
      <td>2790071103004</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>32773</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>-1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>99</th>
      <td>2790071103</td>
      <td>F84</td>
      <td>2790071103009</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>32773</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>-1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>100</th>
      <td>2790071104</td>
      <td>R47</td>
      <td>2790071104002</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>41304</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>3</td>
    </tr>
    <tr>
      <th>101</th>
      <td>2790071104</td>
      <td>F84</td>
      <td>2790071104013</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>41304</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>3</td>
    </tr>
    <tr>
      <th>102</th>
      <td>2790071105</td>
      <td>F80</td>
      <td>2790071105008</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>34207</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>3</td>
    </tr>
    <tr>
      <th>103</th>
      <td>2790133104</td>
      <td>F84</td>
      <td>2790133104006</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>14737</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>104</th>
      <td>2790197104</td>
      <td>F84</td>
      <td>2790197104009</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>12593</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>-1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>105</th>
      <td>2790197105</td>
      <td>F84</td>
      <td>2790197105001</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>12813</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>-1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>106</th>
      <td>2790331104</td>
      <td>F80</td>
      <td>2790331104002</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>14984</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>-1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>107</th>
      <td>2790501104</td>
      <td>F84</td>
      <td>2790501104002</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>6203</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>-1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>108</th>
      <td>2790501105</td>
      <td>F84</td>
      <td>2790501105008</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>6388</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>1</td>
    </tr>
    <tr>
      <th>109</th>
      <td>2790513103</td>
      <td>F80</td>
      <td>2790513103002</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>23143</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>-1</td>
      <td>5</td>
      <td>1</td>
    </tr>
    <tr>
      <th>110</th>
      <td>2790680103</td>
      <td>R47</td>
      <td>2790680103001</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>8538</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>-1</td>
      <td>5</td>
      <td>1</td>
    </tr>
    <tr>
      <th>111</th>
      <td>2790865104</td>
      <td>F84</td>
      <td>2790865104002</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>16780</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>112</th>
      <td>2790944103</td>
      <td>R47</td>
      <td>2790944103003</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>4946</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>-1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>113</th>
      <td>2791090104</td>
      <td>F84</td>
      <td>2791090104002</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>86592</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>114</th>
      <td>2791232102</td>
      <td>F84</td>
      <td>2791232102003</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>1028</td>
      <td>2</td>
      <td>6</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>-1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>115</th>
      <td>2791537103</td>
      <td>F84</td>
      <td>2791537103002</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>31846</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>116</th>
      <td>2791557103</td>
      <td>R47</td>
      <td>2791557103001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>6422</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>5</td>
      <td>3</td>
    </tr>
    <tr>
      <th>117</th>
      <td>2791801101</td>
      <td>R47</td>
      <td>2791801101007</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>11312</td>
      <td>6</td>
      <td>1</td>
      <td>3</td>
      <td>1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>118</th>
      <td>2791969103</td>
      <td>F84</td>
      <td>2791969103003</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>22569</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>119</th>
      <td>2791998103</td>
      <td>F84</td>
      <td>2791998103001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>19786</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>4</td>
    </tr>
    <tr>
      <th>120</th>
      <td>2792167106</td>
      <td>F84</td>
      <td>2792167106001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>11323</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>3</td>
    </tr>
    <tr>
      <th>121</th>
      <td>2792172103</td>
      <td>F80</td>
      <td>2792172103008</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>33420</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>-8</td>
      <td>5</td>
      <td>2</td>
    </tr>
    <tr>
      <th>122</th>
      <td>2792316103</td>
      <td>F84</td>
      <td>2792316103001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>29063</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>123</th>
      <td>2792316103</td>
      <td>F84</td>
      <td>2792316103002</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>29063</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>124</th>
      <td>2792406102</td>
      <td>F84</td>
      <td>2792406102002</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>4845</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>125</th>
      <td>2792406103</td>
      <td>F80</td>
      <td>2792406103002</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>5396</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>-1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>126</th>
      <td>2792452103</td>
      <td>F80</td>
      <td>2792452103001</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>10154</td>
      <td>1</td>
      <td>4</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>-1</td>
      <td>5</td>
      <td>1</td>
    </tr>
    <tr>
      <th>127</th>
      <td>2792672102</td>
      <td>R47</td>
      <td>2792672102002</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>7240</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>5</td>
      <td>1</td>
    </tr>
    <tr>
      <th>128</th>
      <td>2792734104</td>
      <td>F84</td>
      <td>2792734104002</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>10487</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>-1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>129</th>
      <td>2793048102</td>
      <td>F84</td>
      <td>2793048102001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>10195</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>-1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>130</th>
      <td>2793125106</td>
      <td>F80</td>
      <td>2793125106002</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>12879</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>-1</td>
      <td>5</td>
      <td>1</td>
    </tr>
    <tr>
      <th>131</th>
      <td>2793223106</td>
      <td>R47</td>
      <td>2793223106001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>21317</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>3</td>
    </tr>
    <tr>
      <th>132</th>
      <td>2793277105</td>
      <td>F80</td>
      <td>2793277105001</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>23216</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>3</td>
    </tr>
    <tr>
      <th>133</th>
      <td>2793306102</td>
      <td>F84</td>
      <td>2793306102006</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>23141</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>1</td>
    </tr>
    <tr>
      <th>134</th>
      <td>2793568105</td>
      <td>R47</td>
      <td>2793568105001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>11578</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>-1</td>
      <td>-1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>135</th>
      <td>2793836104</td>
      <td>R47</td>
      <td>2793836104002</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>42061</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>3</td>
    </tr>
    <tr>
      <th>136</th>
      <td>2793836104</td>
      <td>F80</td>
      <td>2793836104004</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>42061</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>3</td>
    </tr>
    <tr>
      <th>137</th>
      <td>2794036103</td>
      <td>F84</td>
      <td>2794036103003</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>53802</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>-1</td>
      <td>5</td>
      <td>1</td>
    </tr>
    <tr>
      <th>138</th>
      <td>2794044102</td>
      <td>F84</td>
      <td>2794044102003</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>13114</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>139</th>
      <td>2794430102</td>
      <td>R47</td>
      <td>2794430102013</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>19081</td>
      <td>4</td>
      <td>1</td>
      <td>3</td>
      <td>1</td>
      <td>-1</td>
      <td>-1</td>
      <td>2</td>
      <td>4</td>
    </tr>
    <tr>
      <th>140</th>
      <td>2794519104</td>
      <td>R47</td>
      <td>2794519104001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>8731</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>-1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>141</th>
      <td>2794715103</td>
      <td>F84</td>
      <td>2794715103002</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>7776</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>-1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>142</th>
      <td>2794747102</td>
      <td>F84</td>
      <td>2794747102004</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>13429</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>-1</td>
      <td>5</td>
    </tr>
    <tr>
      <th>143</th>
      <td>2794754104</td>
      <td>F84</td>
      <td>2794754104001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>8356</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>-1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>144</th>
      <td>2794758105</td>
      <td>F84</td>
      <td>2794758105001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>21788</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>145</th>
      <td>2794934113</td>
      <td>F80</td>
      <td>2794934113002</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>11720</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>-8</td>
      <td>-1</td>
      <td>1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>146</th>
      <td>2794934114</td>
      <td>F80</td>
      <td>2794934114001</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>17172</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>-8</td>
      <td>-1</td>
      <td>1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>147</th>
      <td>2795047104</td>
      <td>F80</td>
      <td>2795047104001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>10878</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>1</td>
    </tr>
    <tr>
      <th>148</th>
      <td>2795047104</td>
      <td>F84</td>
      <td>2795047104003</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>10878</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>1</td>
    </tr>
    <tr>
      <th>149</th>
      <td>2795092103</td>
      <td>F84</td>
      <td>2795092103002</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>15852</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>-1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>150</th>
      <td>2795117105</td>
      <td>F84</td>
      <td>2795117105002</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>13563</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>-1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>151</th>
      <td>2795154103</td>
      <td>F84</td>
      <td>2795154103004</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>55895</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>152</th>
      <td>2795162104</td>
      <td>R47</td>
      <td>2795162104005</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>11787</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>-1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>153</th>
      <td>2795162104</td>
      <td>F84</td>
      <td>2795162104019</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>11787</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>-1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>154</th>
      <td>2795536105</td>
      <td>F84</td>
      <td>2795536105005</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>9221</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>155</th>
      <td>2795570101</td>
      <td>F84</td>
      <td>2795570101001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>29711</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>156</th>
      <td>2795640102</td>
      <td>F80</td>
      <td>2795640102003</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>54520</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>-1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>157</th>
      <td>2795640104</td>
      <td>F80</td>
      <td>2795640104003</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>49746</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>-1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>158</th>
      <td>2795647103</td>
      <td>F80</td>
      <td>2795647103001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>9174</td>
      <td>1</td>
      <td>4</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>2</td>
    </tr>
    <tr>
      <th>159</th>
      <td>2795701106</td>
      <td>F80</td>
      <td>2795701106003</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>18734</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>5</td>
      <td>2</td>
    </tr>
    <tr>
      <th>160</th>
      <td>2795788103</td>
      <td>F80</td>
      <td>2795788103001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>22536</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>-1</td>
      <td>-1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>161</th>
      <td>2796449104</td>
      <td>F80</td>
      <td>2796449104001</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>3134</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>5</td>
      <td>1</td>
    </tr>
    <tr>
      <th>162</th>
      <td>2796518104</td>
      <td>F84</td>
      <td>2796518104002</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>19991</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>-1</td>
      <td>5</td>
      <td>2</td>
    </tr>
    <tr>
      <th>163</th>
      <td>2796573104</td>
      <td>F84</td>
      <td>2796573104001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>10271</td>
      <td>1</td>
      <td>6</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>-1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>164</th>
      <td>2796859105</td>
      <td>R47</td>
      <td>2796859105001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>30083</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>5</td>
      <td>1</td>
    </tr>
    <tr>
      <th>165</th>
      <td>2797008104</td>
      <td>R47</td>
      <td>2797008104002</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>30386</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>-1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>166</th>
      <td>2797008106</td>
      <td>R47</td>
      <td>2797008106002</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>34345</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>5</td>
      <td>1</td>
    </tr>
    <tr>
      <th>167</th>
      <td>2797144104</td>
      <td>R47</td>
      <td>2797144104004</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>17896</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>5</td>
      <td>1</td>
    </tr>
    <tr>
      <th>168</th>
      <td>2797188104</td>
      <td>R47</td>
      <td>2797188104001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>12970</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>5</td>
      <td>2</td>
    </tr>
    <tr>
      <th>169</th>
      <td>2797188105</td>
      <td>R47</td>
      <td>2797188105001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>12970</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>5</td>
      <td>2</td>
    </tr>
    <tr>
      <th>170</th>
      <td>2797188106</td>
      <td>R47</td>
      <td>2797188106003</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>13970</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>5</td>
      <td>4</td>
    </tr>
    <tr>
      <th>171</th>
      <td>2797226102</td>
      <td>F84</td>
      <td>2797226102001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>17585</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>3</td>
    </tr>
    <tr>
      <th>172</th>
      <td>2797539103</td>
      <td>R47</td>
      <td>2797539103001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>9273</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>-1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>173</th>
      <td>2797765104</td>
      <td>R47</td>
      <td>2797765104002</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>15289</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>-1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>174</th>
      <td>2797798102</td>
      <td>F84</td>
      <td>2797798102004</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>17299</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>175</th>
      <td>2797874103</td>
      <td>F84</td>
      <td>2797874103003</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>15039</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>176</th>
      <td>2797874105</td>
      <td>F80</td>
      <td>2797874105006</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>8699</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>-1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>177</th>
      <td>2797908103</td>
      <td>F80</td>
      <td>2797908103001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>33946</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>-1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>178</th>
      <td>2797908104</td>
      <td>R47</td>
      <td>2797908104001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>34955</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>5</td>
      <td>1</td>
    </tr>
    <tr>
      <th>179</th>
      <td>2798303103</td>
      <td>F84</td>
      <td>2798303103002</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>17738</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>-1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>180</th>
      <td>2798327101</td>
      <td>R47</td>
      <td>2798327101003</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>11627</td>
      <td>5</td>
      <td>1</td>
      <td>3</td>
      <td>1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>181</th>
      <td>2798428104</td>
      <td>F80</td>
      <td>2798428104001</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>21104</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>-7</td>
      <td>-1</td>
      <td>5</td>
      <td>3</td>
    </tr>
    <tr>
      <th>182</th>
      <td>2798498103</td>
      <td>F80</td>
      <td>2798498103001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>37233</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>5</td>
      <td>1</td>
    </tr>
    <tr>
      <th>183</th>
      <td>2798587101</td>
      <td>F80</td>
      <td>2798587101021</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>13693</td>
      <td>5</td>
      <td>1</td>
      <td>3</td>
      <td>1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>184</th>
      <td>2798845103</td>
      <td>F80</td>
      <td>2798845103003</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>30293</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>185</th>
      <td>2798926102</td>
      <td>F80</td>
      <td>2798926102003</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>7052</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>4</td>
    </tr>
    <tr>
      <th>186</th>
      <td>2798961103</td>
      <td>F84</td>
      <td>2798961103003</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>21091</td>
      <td>1</td>
      <td>6</td>
      <td>2</td>
      <td>2</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>187</th>
      <td>2799058103</td>
      <td>F84</td>
      <td>2799058103001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>21086</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>-1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>188</th>
      <td>2799072105</td>
      <td>F80</td>
      <td>2799072105005</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>14389</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>189</th>
      <td>2799132103</td>
      <td>F84</td>
      <td>2799132103004</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>14936</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>2</td>
    </tr>
    <tr>
      <th>190</th>
      <td>2799152102</td>
      <td>F84</td>
      <td>2799152102005</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>5766</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>191</th>
      <td>2799192103</td>
      <td>F84</td>
      <td>2799192103001</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>18344</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>4</td>
      <td>2</td>
    </tr>
    <tr>
      <th>192</th>
      <td>2799284104</td>
      <td>F84</td>
      <td>2799284104003</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>14654</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>-1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>193</th>
      <td>2799305102</td>
      <td>F84</td>
      <td>2799305102002</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>15536</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>194</th>
      <td>2799383102</td>
      <td>F80</td>
      <td>2799383102001</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>10552</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>3</td>
    </tr>
    <tr>
      <th>195</th>
      <td>2799484103</td>
      <td>F84</td>
      <td>2799484103005</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>40934</td>
      <td>1</td>
      <td>6</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>-1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>196</th>
      <td>2799508103</td>
      <td>F84</td>
      <td>2799508103001</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>12890</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>197</th>
      <td>2799596104</td>
      <td>R47</td>
      <td>2799596104002</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>67064</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>-1</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-04bdbe60-d51a-4e5f-a94f-18b995b1e7d7')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-04bdbe60-d51a-4e5f-a94f-18b995b1e7d7 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-04bdbe60-d51a-4e5f-a94f-18b995b1e7d7');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>


    <div id="df-e4695278-4cb8-42da-855c-0723aa635415">
      <button class="colab-df-quickchart" onclick="quickchart('df-e4695278-4cb8-42da-855c-0723aa635415')"
                title="Suggest charts"
                style="display:none;">

<svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
     width="24px">
    <g>
        <path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z"/>
    </g>
</svg>
      </button>

<style>
  .colab-df-quickchart {
      --bg-color: #E8F0FE;
      --fill-color: #1967D2;
      --hover-bg-color: #E2EBFA;
      --hover-fill-color: #174EA6;
      --disabled-fill-color: #AAA;
      --disabled-bg-color: #DDD;
  }

  [theme=dark] .colab-df-quickchart {
      --bg-color: #3B4455;
      --fill-color: #D2E3FC;
      --hover-bg-color: #434B5C;
      --hover-fill-color: #FFFFFF;
      --disabled-bg-color: #3B4455;
      --disabled-fill-color: #666;
  }

  .colab-df-quickchart {
    background-color: var(--bg-color);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: none;
    fill: var(--fill-color);
    height: 32px;
    padding: 0;
    width: 32px;
  }

  .colab-df-quickchart:hover {
    background-color: var(--hover-bg-color);
    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);
    fill: var(--button-hover-fill-color);
  }

  .colab-df-quickchart-complete:disabled,
  .colab-df-quickchart-complete:disabled:hover {
    background-color: var(--disabled-bg-color);
    fill: var(--disabled-fill-color);
    box-shadow: none;
  }

  .colab-df-spinner {
    border: 2px solid var(--fill-color);
    border-color: transparent;
    border-bottom-color: var(--fill-color);
    animation:
      spin 1s steps(1) infinite;
  }

  @keyframes spin {
    0% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
      border-left-color: var(--fill-color);
    }
    20% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    30% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
      border-right-color: var(--fill-color);
    }
    40% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    60% {
      border-color: transparent;
      border-right-color: var(--fill-color);
    }
    80% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-bottom-color: var(--fill-color);
    }
    90% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
    }
  }
</style>

      <script>
        async function quickchart(key) {
          const quickchartButtonEl =
            document.querySelector('#' + key + ' button');
          quickchartButtonEl.disabled = true;  // To prevent multiple clicks.
          quickchartButtonEl.classList.add('colab-df-spinner');
          try {
            const charts = await google.colab.kernel.invokeFunction(
                'suggestCharts', [key], {});
          } catch (error) {
            console.error('Error during call to suggestCharts:', error);
          }
          quickchartButtonEl.classList.remove('colab-df-spinner');
          quickchartButtonEl.classList.add('colab-df-quickchart-complete');
        }
        (() => {
          let quickchartButtonEl =
            document.querySelector('#df-e4695278-4cb8-42da-855c-0723aa635415 button');
          quickchartButtonEl.style.display =
            google.colab.kernel.accessAllowed ? 'block' : 'none';
        })();
      </script>
    </div>

    </div>
  </div>





```python
df['INSURC22'].map({1: 'Private Health Insurance', 2: 'Public Health Insurance', 3: 'Uninsured'})
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>INSURC22</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>29</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>30</th>
      <td>NaN</td>
    </tr>
    <tr>
      <th>31</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>32</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>33</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>34</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>35</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>36</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>37</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>38</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>39</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>40</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>41</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>42</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>43</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>44</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>45</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>46</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>47</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>48</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>49</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>50</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>51</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>52</th>
      <td>NaN</td>
    </tr>
    <tr>
      <th>53</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>54</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>55</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>56</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>57</th>
      <td>NaN</td>
    </tr>
    <tr>
      <th>58</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>59</th>
      <td>Uninsured</td>
    </tr>
    <tr>
      <th>60</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>61</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>62</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>63</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>64</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>65</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>66</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>67</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>68</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>69</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>70</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>71</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>72</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>73</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>74</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>75</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>76</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>77</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>78</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>79</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>80</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>81</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>82</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>83</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>84</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>85</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>86</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>87</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>88</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>89</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>90</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>91</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>92</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>93</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>94</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>95</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>96</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>97</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>98</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>99</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>100</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>101</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>102</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>103</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>104</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>105</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>106</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>107</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>108</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>109</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>110</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>111</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>112</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>113</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>114</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>115</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>116</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>117</th>
      <td>NaN</td>
    </tr>
    <tr>
      <th>118</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>119</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>120</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>121</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>122</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>123</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>124</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>125</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>126</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>127</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>128</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>129</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>130</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>131</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>132</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>133</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>134</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>135</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>136</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>137</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>138</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>139</th>
      <td>NaN</td>
    </tr>
    <tr>
      <th>140</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>141</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>142</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>143</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>144</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>145</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>146</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>147</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>148</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>149</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>150</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>151</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>152</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>153</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>154</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>155</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>156</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>157</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>158</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>159</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>160</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>161</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>162</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>163</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>164</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>165</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>166</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>167</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>168</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>169</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>170</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>171</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>172</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>173</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>174</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>175</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>176</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>177</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>178</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>179</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>180</th>
      <td>NaN</td>
    </tr>
    <tr>
      <th>181</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>182</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>183</th>
      <td>NaN</td>
    </tr>
    <tr>
      <th>184</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>185</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>186</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>187</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>188</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>189</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>190</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>191</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>192</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>193</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>194</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>195</th>
      <td>Private Health Insurance</td>
    </tr>
    <tr>
      <th>196</th>
      <td>Public Health Insurance</td>
    </tr>
    <tr>
      <th>197</th>
      <td>Private Health Insurance</td>
    </tr>
  </tbody>
</table>
</div><br><label><b>dtype:</b> object</label>




```python
df['RACEV1X'].map({1: 'White', 2: 'Black', 4: 'Asian', 6: 'Multiple races'})
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>RACEV1X</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>White</td>
    </tr>
    <tr>
      <th>1</th>
      <td>White</td>
    </tr>
    <tr>
      <th>2</th>
      <td>White</td>
    </tr>
    <tr>
      <th>3</th>
      <td>White</td>
    </tr>
    <tr>
      <th>4</th>
      <td>White</td>
    </tr>
    <tr>
      <th>5</th>
      <td>White</td>
    </tr>
    <tr>
      <th>6</th>
      <td>White</td>
    </tr>
    <tr>
      <th>7</th>
      <td>White</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Multiple races</td>
    </tr>
    <tr>
      <th>9</th>
      <td>White</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Black</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Black</td>
    </tr>
    <tr>
      <th>12</th>
      <td>White</td>
    </tr>
    <tr>
      <th>13</th>
      <td>White</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Asian</td>
    </tr>
    <tr>
      <th>15</th>
      <td>White</td>
    </tr>
    <tr>
      <th>16</th>
      <td>White</td>
    </tr>
    <tr>
      <th>17</th>
      <td>White</td>
    </tr>
    <tr>
      <th>18</th>
      <td>White</td>
    </tr>
    <tr>
      <th>19</th>
      <td>White</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Multiple races</td>
    </tr>
    <tr>
      <th>21</th>
      <td>White</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Multiple races</td>
    </tr>
    <tr>
      <th>23</th>
      <td>White</td>
    </tr>
    <tr>
      <th>24</th>
      <td>White</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Black</td>
    </tr>
    <tr>
      <th>26</th>
      <td>White</td>
    </tr>
    <tr>
      <th>27</th>
      <td>White</td>
    </tr>
    <tr>
      <th>28</th>
      <td>White</td>
    </tr>
    <tr>
      <th>29</th>
      <td>White</td>
    </tr>
    <tr>
      <th>30</th>
      <td>White</td>
    </tr>
    <tr>
      <th>31</th>
      <td>White</td>
    </tr>
    <tr>
      <th>32</th>
      <td>White</td>
    </tr>
    <tr>
      <th>33</th>
      <td>White</td>
    </tr>
    <tr>
      <th>34</th>
      <td>Black</td>
    </tr>
    <tr>
      <th>35</th>
      <td>White</td>
    </tr>
    <tr>
      <th>36</th>
      <td>White</td>
    </tr>
    <tr>
      <th>37</th>
      <td>White</td>
    </tr>
    <tr>
      <th>38</th>
      <td>White</td>
    </tr>
    <tr>
      <th>39</th>
      <td>White</td>
    </tr>
    <tr>
      <th>40</th>
      <td>White</td>
    </tr>
    <tr>
      <th>41</th>
      <td>White</td>
    </tr>
    <tr>
      <th>42</th>
      <td>White</td>
    </tr>
    <tr>
      <th>43</th>
      <td>White</td>
    </tr>
    <tr>
      <th>44</th>
      <td>White</td>
    </tr>
    <tr>
      <th>45</th>
      <td>White</td>
    </tr>
    <tr>
      <th>46</th>
      <td>Asian</td>
    </tr>
    <tr>
      <th>47</th>
      <td>White</td>
    </tr>
    <tr>
      <th>48</th>
      <td>White</td>
    </tr>
    <tr>
      <th>49</th>
      <td>White</td>
    </tr>
    <tr>
      <th>50</th>
      <td>White</td>
    </tr>
    <tr>
      <th>51</th>
      <td>White</td>
    </tr>
    <tr>
      <th>52</th>
      <td>Asian</td>
    </tr>
    <tr>
      <th>53</th>
      <td>White</td>
    </tr>
    <tr>
      <th>54</th>
      <td>Multiple races</td>
    </tr>
    <tr>
      <th>55</th>
      <td>White</td>
    </tr>
    <tr>
      <th>56</th>
      <td>White</td>
    </tr>
    <tr>
      <th>57</th>
      <td>White</td>
    </tr>
    <tr>
      <th>58</th>
      <td>Asian</td>
    </tr>
    <tr>
      <th>59</th>
      <td>White</td>
    </tr>
    <tr>
      <th>60</th>
      <td>White</td>
    </tr>
    <tr>
      <th>61</th>
      <td>White</td>
    </tr>
    <tr>
      <th>62</th>
      <td>White</td>
    </tr>
    <tr>
      <th>63</th>
      <td>White</td>
    </tr>
    <tr>
      <th>64</th>
      <td>White</td>
    </tr>
    <tr>
      <th>65</th>
      <td>White</td>
    </tr>
    <tr>
      <th>66</th>
      <td>White</td>
    </tr>
    <tr>
      <th>67</th>
      <td>White</td>
    </tr>
    <tr>
      <th>68</th>
      <td>White</td>
    </tr>
    <tr>
      <th>69</th>
      <td>Multiple races</td>
    </tr>
    <tr>
      <th>70</th>
      <td>Multiple races</td>
    </tr>
    <tr>
      <th>71</th>
      <td>White</td>
    </tr>
    <tr>
      <th>72</th>
      <td>White</td>
    </tr>
    <tr>
      <th>73</th>
      <td>Multiple races</td>
    </tr>
    <tr>
      <th>74</th>
      <td>Multiple races</td>
    </tr>
    <tr>
      <th>75</th>
      <td>White</td>
    </tr>
    <tr>
      <th>76</th>
      <td>White</td>
    </tr>
    <tr>
      <th>77</th>
      <td>White</td>
    </tr>
    <tr>
      <th>78</th>
      <td>White</td>
    </tr>
    <tr>
      <th>79</th>
      <td>White</td>
    </tr>
    <tr>
      <th>80</th>
      <td>White</td>
    </tr>
    <tr>
      <th>81</th>
      <td>White</td>
    </tr>
    <tr>
      <th>82</th>
      <td>White</td>
    </tr>
    <tr>
      <th>83</th>
      <td>White</td>
    </tr>
    <tr>
      <th>84</th>
      <td>Black</td>
    </tr>
    <tr>
      <th>85</th>
      <td>White</td>
    </tr>
    <tr>
      <th>86</th>
      <td>White</td>
    </tr>
    <tr>
      <th>87</th>
      <td>White</td>
    </tr>
    <tr>
      <th>88</th>
      <td>White</td>
    </tr>
    <tr>
      <th>89</th>
      <td>White</td>
    </tr>
    <tr>
      <th>90</th>
      <td>White</td>
    </tr>
    <tr>
      <th>91</th>
      <td>White</td>
    </tr>
    <tr>
      <th>92</th>
      <td>White</td>
    </tr>
    <tr>
      <th>93</th>
      <td>White</td>
    </tr>
    <tr>
      <th>94</th>
      <td>White</td>
    </tr>
    <tr>
      <th>95</th>
      <td>White</td>
    </tr>
    <tr>
      <th>96</th>
      <td>White</td>
    </tr>
    <tr>
      <th>97</th>
      <td>White</td>
    </tr>
    <tr>
      <th>98</th>
      <td>White</td>
    </tr>
    <tr>
      <th>99</th>
      <td>White</td>
    </tr>
    <tr>
      <th>100</th>
      <td>White</td>
    </tr>
    <tr>
      <th>101</th>
      <td>White</td>
    </tr>
    <tr>
      <th>102</th>
      <td>White</td>
    </tr>
    <tr>
      <th>103</th>
      <td>White</td>
    </tr>
    <tr>
      <th>104</th>
      <td>White</td>
    </tr>
    <tr>
      <th>105</th>
      <td>White</td>
    </tr>
    <tr>
      <th>106</th>
      <td>White</td>
    </tr>
    <tr>
      <th>107</th>
      <td>White</td>
    </tr>
    <tr>
      <th>108</th>
      <td>White</td>
    </tr>
    <tr>
      <th>109</th>
      <td>White</td>
    </tr>
    <tr>
      <th>110</th>
      <td>Black</td>
    </tr>
    <tr>
      <th>111</th>
      <td>White</td>
    </tr>
    <tr>
      <th>112</th>
      <td>White</td>
    </tr>
    <tr>
      <th>113</th>
      <td>White</td>
    </tr>
    <tr>
      <th>114</th>
      <td>Multiple races</td>
    </tr>
    <tr>
      <th>115</th>
      <td>White</td>
    </tr>
    <tr>
      <th>116</th>
      <td>White</td>
    </tr>
    <tr>
      <th>117</th>
      <td>White</td>
    </tr>
    <tr>
      <th>118</th>
      <td>White</td>
    </tr>
    <tr>
      <th>119</th>
      <td>White</td>
    </tr>
    <tr>
      <th>120</th>
      <td>White</td>
    </tr>
    <tr>
      <th>121</th>
      <td>Black</td>
    </tr>
    <tr>
      <th>122</th>
      <td>Black</td>
    </tr>
    <tr>
      <th>123</th>
      <td>Black</td>
    </tr>
    <tr>
      <th>124</th>
      <td>White</td>
    </tr>
    <tr>
      <th>125</th>
      <td>White</td>
    </tr>
    <tr>
      <th>126</th>
      <td>Asian</td>
    </tr>
    <tr>
      <th>127</th>
      <td>Black</td>
    </tr>
    <tr>
      <th>128</th>
      <td>Black</td>
    </tr>
    <tr>
      <th>129</th>
      <td>White</td>
    </tr>
    <tr>
      <th>130</th>
      <td>White</td>
    </tr>
    <tr>
      <th>131</th>
      <td>White</td>
    </tr>
    <tr>
      <th>132</th>
      <td>White</td>
    </tr>
    <tr>
      <th>133</th>
      <td>White</td>
    </tr>
    <tr>
      <th>134</th>
      <td>White</td>
    </tr>
    <tr>
      <th>135</th>
      <td>White</td>
    </tr>
    <tr>
      <th>136</th>
      <td>White</td>
    </tr>
    <tr>
      <th>137</th>
      <td>White</td>
    </tr>
    <tr>
      <th>138</th>
      <td>Black</td>
    </tr>
    <tr>
      <th>139</th>
      <td>White</td>
    </tr>
    <tr>
      <th>140</th>
      <td>White</td>
    </tr>
    <tr>
      <th>141</th>
      <td>White</td>
    </tr>
    <tr>
      <th>142</th>
      <td>White</td>
    </tr>
    <tr>
      <th>143</th>
      <td>White</td>
    </tr>
    <tr>
      <th>144</th>
      <td>White</td>
    </tr>
    <tr>
      <th>145</th>
      <td>White</td>
    </tr>
    <tr>
      <th>146</th>
      <td>White</td>
    </tr>
    <tr>
      <th>147</th>
      <td>White</td>
    </tr>
    <tr>
      <th>148</th>
      <td>White</td>
    </tr>
    <tr>
      <th>149</th>
      <td>White</td>
    </tr>
    <tr>
      <th>150</th>
      <td>White</td>
    </tr>
    <tr>
      <th>151</th>
      <td>White</td>
    </tr>
    <tr>
      <th>152</th>
      <td>White</td>
    </tr>
    <tr>
      <th>153</th>
      <td>White</td>
    </tr>
    <tr>
      <th>154</th>
      <td>White</td>
    </tr>
    <tr>
      <th>155</th>
      <td>White</td>
    </tr>
    <tr>
      <th>156</th>
      <td>White</td>
    </tr>
    <tr>
      <th>157</th>
      <td>White</td>
    </tr>
    <tr>
      <th>158</th>
      <td>Asian</td>
    </tr>
    <tr>
      <th>159</th>
      <td>White</td>
    </tr>
    <tr>
      <th>160</th>
      <td>White</td>
    </tr>
    <tr>
      <th>161</th>
      <td>White</td>
    </tr>
    <tr>
      <th>162</th>
      <td>Black</td>
    </tr>
    <tr>
      <th>163</th>
      <td>Multiple races</td>
    </tr>
    <tr>
      <th>164</th>
      <td>White</td>
    </tr>
    <tr>
      <th>165</th>
      <td>White</td>
    </tr>
    <tr>
      <th>166</th>
      <td>White</td>
    </tr>
    <tr>
      <th>167</th>
      <td>White</td>
    </tr>
    <tr>
      <th>168</th>
      <td>White</td>
    </tr>
    <tr>
      <th>169</th>
      <td>White</td>
    </tr>
    <tr>
      <th>170</th>
      <td>White</td>
    </tr>
    <tr>
      <th>171</th>
      <td>Black</td>
    </tr>
    <tr>
      <th>172</th>
      <td>Black</td>
    </tr>
    <tr>
      <th>173</th>
      <td>White</td>
    </tr>
    <tr>
      <th>174</th>
      <td>White</td>
    </tr>
    <tr>
      <th>175</th>
      <td>White</td>
    </tr>
    <tr>
      <th>176</th>
      <td>White</td>
    </tr>
    <tr>
      <th>177</th>
      <td>White</td>
    </tr>
    <tr>
      <th>178</th>
      <td>White</td>
    </tr>
    <tr>
      <th>179</th>
      <td>White</td>
    </tr>
    <tr>
      <th>180</th>
      <td>White</td>
    </tr>
    <tr>
      <th>181</th>
      <td>White</td>
    </tr>
    <tr>
      <th>182</th>
      <td>White</td>
    </tr>
    <tr>
      <th>183</th>
      <td>White</td>
    </tr>
    <tr>
      <th>184</th>
      <td>Black</td>
    </tr>
    <tr>
      <th>185</th>
      <td>White</td>
    </tr>
    <tr>
      <th>186</th>
      <td>Multiple races</td>
    </tr>
    <tr>
      <th>187</th>
      <td>White</td>
    </tr>
    <tr>
      <th>188</th>
      <td>White</td>
    </tr>
    <tr>
      <th>189</th>
      <td>White</td>
    </tr>
    <tr>
      <th>190</th>
      <td>White</td>
    </tr>
    <tr>
      <th>191</th>
      <td>Black</td>
    </tr>
    <tr>
      <th>192</th>
      <td>White</td>
    </tr>
    <tr>
      <th>193</th>
      <td>White</td>
    </tr>
    <tr>
      <th>194</th>
      <td>White</td>
    </tr>
    <tr>
      <th>195</th>
      <td>Multiple races</td>
    </tr>
    <tr>
      <th>196</th>
      <td>White</td>
    </tr>
    <tr>
      <th>197</th>
      <td>White</td>
    </tr>
  </tbody>
</table>
</div><br><label><b>dtype:</b> object</label>




```python
df['RTHLTH53'].map({-1: 'N/A', 1: 'Excellent', 2: 'Very Good', 3: 'Good', 4: 'Fair', 5: 'Poor'})
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>RTHLTH53</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Fair</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Poor</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Poor</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Fair</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Fair</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Fair</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Fair</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Fair</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Fair</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Poor</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>29</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>30</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>31</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>32</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>33</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>34</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>35</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>36</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>37</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>38</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>39</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>40</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>41</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>42</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>43</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>44</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>45</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>46</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>47</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>48</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>49</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>50</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>51</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>52</th>
      <td>Fair</td>
    </tr>
    <tr>
      <th>53</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>54</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>55</th>
      <td>Poor</td>
    </tr>
    <tr>
      <th>56</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>57</th>
      <td>N/A</td>
    </tr>
    <tr>
      <th>58</th>
      <td>Fair</td>
    </tr>
    <tr>
      <th>59</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>60</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>61</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>62</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>63</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>64</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>65</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>66</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>67</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>68</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>69</th>
      <td>Fair</td>
    </tr>
    <tr>
      <th>70</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>71</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>72</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>73</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>74</th>
      <td>Fair</td>
    </tr>
    <tr>
      <th>75</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>76</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>77</th>
      <td>Fair</td>
    </tr>
    <tr>
      <th>78</th>
      <td>Fair</td>
    </tr>
    <tr>
      <th>79</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>80</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>81</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>82</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>83</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>84</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>85</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>86</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>87</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>88</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>89</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>90</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>91</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>92</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>93</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>94</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>95</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>96</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>97</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>98</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>99</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>100</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>101</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>102</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>103</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>104</th>
      <td>Fair</td>
    </tr>
    <tr>
      <th>105</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>106</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>107</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>108</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>109</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>110</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>111</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>112</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>113</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>114</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>115</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>116</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>117</th>
      <td>Fair</td>
    </tr>
    <tr>
      <th>118</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>119</th>
      <td>Fair</td>
    </tr>
    <tr>
      <th>120</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>121</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>122</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>123</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>124</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>125</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>126</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>127</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>128</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>129</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>130</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>131</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>132</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>133</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>134</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>135</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>136</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>137</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>138</th>
      <td>Fair</td>
    </tr>
    <tr>
      <th>139</th>
      <td>Fair</td>
    </tr>
    <tr>
      <th>140</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>141</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>142</th>
      <td>Poor</td>
    </tr>
    <tr>
      <th>143</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>144</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>145</th>
      <td>Fair</td>
    </tr>
    <tr>
      <th>146</th>
      <td>Fair</td>
    </tr>
    <tr>
      <th>147</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>148</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>149</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>150</th>
      <td>Fair</td>
    </tr>
    <tr>
      <th>151</th>
      <td>Fair</td>
    </tr>
    <tr>
      <th>152</th>
      <td>Fair</td>
    </tr>
    <tr>
      <th>153</th>
      <td>Fair</td>
    </tr>
    <tr>
      <th>154</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>155</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>156</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>157</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>158</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>159</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>160</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>161</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>162</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>163</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>164</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>165</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>166</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>167</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>168</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>169</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>170</th>
      <td>Fair</td>
    </tr>
    <tr>
      <th>171</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>172</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>173</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>174</th>
      <td>Fair</td>
    </tr>
    <tr>
      <th>175</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>176</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>177</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>178</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>179</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>180</th>
      <td>Fair</td>
    </tr>
    <tr>
      <th>181</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>182</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>183</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>184</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>185</th>
      <td>Fair</td>
    </tr>
    <tr>
      <th>186</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>187</th>
      <td>Excellent</td>
    </tr>
    <tr>
      <th>188</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>189</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>190</th>
      <td>Fair</td>
    </tr>
    <tr>
      <th>191</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>192</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>193</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>194</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>195</th>
      <td>Very Good</td>
    </tr>
    <tr>
      <th>196</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>197</th>
      <td>Excellent</td>
    </tr>
  </tbody>
</table>
</div><br><label><b>dtype:</b> object</label>




```python
# prompt: describe the dataframe with mapped labels

import pandas as pd

# Assuming 'df' is your DataFrame with the mapped columns
# Replace 'df' with the actual variable name if it's different

# Display descriptive statistics for the mapped columns
print(df.describe(include='all'))


# Alternatively, if you want to describe specific mapped columns,
# you can specify them:
# For example:
print(df[['Insurance Type', 'Race', 'Health Status', 'English Proficiency']].describe(include='all'))

```

             DUPERSID ICD10CDX       CONDIDX  IPCOND  OPCOND  OBCOND  HHCOND  \
    count         198      198           198     198     198     198     198   
    unique        NaN        3           NaN     NaN     NaN     NaN     NaN   
    top           NaN      F84           NaN     NaN     NaN     NaN     NaN   
    freq          NaN      106           NaN     NaN     NaN     NaN     NaN   
    mean   2698698762      NaN 2698698762085       2       2       1       2   
    std     123605199      NaN  123605198695       0       0       0       0   
    min    2460398101      NaN 2460398101017       1       1       1       1   
    25%    2681309353      NaN 2681309353001       2       2       1       2   
    50%    2790071103      NaN 2790071103006       2       2       1       2   
    75%    2795047104      NaN 2795047104002       2       2       1       2   
    max    2799596104      NaN 2799596104002       2       2       2       2   
    
            PERWT22F  INSURC22  RACEV1X  RACEBX  MCREV22  CHTHER42  CHSRHB42  \
    count        198       198      198     198      198       198       198   
    unique       NaN       NaN      NaN     NaN      NaN       NaN       NaN   
    top          NaN       NaN      NaN     NaN      NaN       NaN       NaN   
    freq         NaN       NaN      NaN     NaN      NaN       NaN       NaN   
    mean       17425         2        1       3        2         1        -0   
    std        12936         1        1       1        0         1         1   
    min            0         1        1       1        1        -8        -8   
    25%         8804         1        1       3        2         1        -1   
    50%        13951         1        1       3        2         1        -1   
    75%        22373         2        1       3        2         1         1   
    max        86592         6        6       3        2         2         2   
    
            HWELLSPK  RTHLTH53            Insurance Type   Race Health Status  \
    count        198       198                       191    198           198   
    unique       NaN       NaN                         3      4             6   
    top          NaN       NaN  Private Health Insurance  White     Excellent   
    freq         NaN       NaN                       103    163            56   
    mean           1         2                       NaN    NaN           NaN   
    std            3         1                       NaN    NaN           NaN   
    min           -1        -1                       NaN    NaN           NaN   
    25%           -1         1                       NaN    NaN           NaN   
    50%           -1         2                       NaN    NaN           NaN   
    75%            5         3                       NaN    NaN           NaN   
    max            5         5                       NaN    NaN           NaN   
    
           English Proficiency  
    count                   16  
    unique                   3  
    top              Very well  
    freq                    13  
    mean                   NaN  
    std                    NaN  
    min                    NaN  
    25%                    NaN  
    50%                    NaN  
    75%                    NaN  
    max                    NaN  
                      Insurance Type   Race Health Status English Proficiency
    count                        191    198           198                  16
    unique                         3      4             6                   3
    top     Private Health Insurance  White     Excellent           Very well
    freq                         103    163            56                  13



```python
df['English Proficiency'] = df['HWELLSPK'].map({1: 'Very well', 2: 'Well', 3: 'Not well', 4: 'Not at all'})
```


```python
# Or describe specific mapped columns:
print(df[['Insurance Type', 'Race', 'Health Status', 'English Proficiency']].describe(include='all'))

# Now you can describe the DataFrame with the mapped columns
print(df.describe(include='all'))

```

                      Insurance Type   Race Health Status English Proficiency
    count                        191    198           198                  16
    unique                         3      4             6                   3
    top     Private Health Insurance  White     Excellent           Very well
    freq                         103    163            56                  13
             DUPERSID ICD10CDX       CONDIDX  IPCOND  OPCOND  OBCOND  HHCOND  \
    count         198      198           198     198     198     198     198   
    unique        NaN        3           NaN     NaN     NaN     NaN     NaN   
    top           NaN      F84           NaN     NaN     NaN     NaN     NaN   
    freq          NaN      106           NaN     NaN     NaN     NaN     NaN   
    mean   2698698762      NaN 2698698762085       2       2       1       2   
    std     123605199      NaN  123605198695       0       0       0       0   
    min    2460398101      NaN 2460398101017       1       1       1       1   
    25%    2681309353      NaN 2681309353001       2       2       1       2   
    50%    2790071103      NaN 2790071103006       2       2       1       2   
    75%    2795047104      NaN 2795047104002       2       2       1       2   
    max    2799596104      NaN 2799596104002       2       2       2       2   
    
            PERWT22F  INSURC22  RACEV1X  RACEBX  MCREV22  CHTHER42  CHSRHB42  \
    count        198       198      198     198      198       198       198   
    unique       NaN       NaN      NaN     NaN      NaN       NaN       NaN   
    top          NaN       NaN      NaN     NaN      NaN       NaN       NaN   
    freq         NaN       NaN      NaN     NaN      NaN       NaN       NaN   
    mean       17425         2        1       3        2         1        -0   
    std        12936         1        1       1        0         1         1   
    min            0         1        1       1        1        -8        -8   
    25%         8804         1        1       3        2         1        -1   
    50%        13951         1        1       3        2         1        -1   
    75%        22373         2        1       3        2         1         1   
    max        86592         6        6       3        2         2         2   
    
            HWELLSPK  RTHLTH53            Insurance Type   Race Health Status  \
    count        198       198                       191    198           198   
    unique       NaN       NaN                         3      4             6   
    top          NaN       NaN  Private Health Insurance  White     Excellent   
    freq         NaN       NaN                       103    163            56   
    mean           1         2                       NaN    NaN           NaN   
    std            3         1                       NaN    NaN           NaN   
    min           -1        -1                       NaN    NaN           NaN   
    25%           -1         1                       NaN    NaN           NaN   
    50%           -1         2                       NaN    NaN           NaN   
    75%            5         3                       NaN    NaN           NaN   
    max            5         5                       NaN    NaN           NaN   
    
           English Proficiency  
    count                   16  
    unique                   3  
    top              Very well  
    freq                    13  
    mean                   NaN  
    std                    NaN  
    min                    NaN  
    25%                    NaN  
    50%                    NaN  
    75%                    NaN  
    max                    NaN  



```python
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt


```


```python
insurance_mapping = {1: 'Private Health Insurance', 2: 'Public Health Insurance', 3: 'Uninsured'}
race_mapping = {1: 'White', 2: 'Black', 4: 'Asian', 6: 'Multiple races'}
health_status_mapping = {-1: 'N/A', 1: 'Excellent', 2: 'Very Good', 3: 'Good', 4: 'Fair', 5: 'Poor'}
```


```python
ct = pd.crosstab(df['INSURC22'], df['RACEV1X'], df['HHCOND'],
                 aggfunc= "sum",
                 normalize= 'index')

print(ct)
```

    RACEV1X   1  2  4  6
    INSURC22            
    1         1  0  0  0
    2         1  0  0  0
    3         1  0  0  0
    4         1  0  0  0
    5         1  0  0  0
    6         1  0  0  0


##Research Questions and Visualizations
How do insurance coverage, race, and English proficiency affect health outcomes for individuals diagnosed with speech and language disorders?

Primary Hypothesis: Individuals with Medicaid or no insurance will report worse health outcomes compared to those with private insurance.

This project aims to provide the following analyses: Descriptive stats will describe the frequency of speech

Descriptive statistics will describe the frequency of speech codes throughout the data set by insurance type, race, and English proficiency and provide a summary of health outcomes.
Chi-square test will see if there is a significant relationship between insurance type, race, English proficiency and health outcomes
Analysis of Variance (ANOVA) will compare mean health outcomes by insurance type, race, and English proficiency


```python
#Frequency by Race
plt.figure(figsize=(8, 5))
sns.countplot(data=df,
              x='Race',
              order=df['Race'].value_counts().index,
              palette=sns.color_palette("Set2"),
              stat='percent',
              legend='auto')
plt.title("Frequency of Speech/Language Disorders by Race")
plt.xlabel("Race")
plt.ylabel("Count")
plt.show()
```


    
![png](MEPS_files/MEPS_38_0.png)
    


The results and correlations having to do with race should be interpreted with caution as there is a higher amount included in the sample. This sample is unprepresentative of the population.


```python
#Frequency by insurance type
plt.figure(figsize=(10, 6))
sns.countplot(data=df, x='Insurance Type',
              order=df['Insurance Type'].value_counts().index,
              hue_order=df['Insurance Type'].value_counts().index,
              palette='Set2',
              stat='percent',
              color=
              legend='auto')
plt.title("Frequency of Speech/Language Disorders by Insurance Type")
plt.xlabel("Insurance Type")
plt.ylabel("Percentage")
plt.legend(title="Insurance Type")
plt.show()
```


    
![png](MEPS_files/MEPS_40_0.png)
    



```python
#Frequency by diagnosis code
plt.figure(figsize=(10, 6))
sns.countplot(data=df,
              x='ICD10CDX',
              order=df['ICD10CDX'].value_counts().index,
              palette=sns.color_palette("Set2"),
              stat='count',
              legend='auto')
plt.title("Frequency of Speech/Language Disorders by Diagnosis Code")
plt.xlabel("ICD-10 Code")
plt.legend(title="Diagnosis Code")
plt.ylabel("Count")
plt.show()
```


    
![png](MEPS_files/MEPS_41_0.png)
    


The most frequent diagnosis code is F84 for Autism Spectrum Disorder, followed by F80 for Specific Developmental Disorders of Speech and Language (articulation, expressive, receptive), and R47 for other speech disturbances such as Dysarthira and Aphasia is the least frequently occuring code.  


```python
#Frequency of Insurance Type by Race
plt.figure(figsize=(10, 6))
sns.countplot(data=df, x='Race', hue='Insurance Type', palette='Set2')
plt.title("Insurance Type Distribution by Race")
plt.xlabel("Race")
plt.ylabel("Count")
plt.legend(title="Insurance Type")
plt.tight_layout()
plt.show()
```


    
![png](MEPS_files/MEPS_43_0.png)
    



```python
# Define desired ascending order for Health Status
health_order = ['Excellent', 'Very Good', 'Good', 'Fair', 'Poor']

# Convert to ordered categorical type
df['Health Status'] = pd.Categorical(df['Health Status'], categories=health_order, ordered=True)

# Plot with reordered y-axis
plt.figure(figsize=(10, 6))
sns.boxplot(data=df, x='Insurance Type',
            y='Health Status',
            orient='v',
            palette='Set2', saturation=1,legend='auto')
plt.title("Health Outcomes by Insurance Type")
plt.xlabel("Insurance Type")
plt.ylabel("Health Outcome Score")
plt.show()

```


    
![png](MEPS_files/MEPS_44_0.png)
    


According to this graph, individuals with private health insurance rate their health from excellent-good, while public health inssurees report very good-good.


```python
# Chi-square test will see if there is a significant relationship between insurance type and health status

import pandas as pd
from scipy.stats import chi2_contingency


# Create a contingency table
contingency_table = pd.crosstab(df['INSURC22'], df['HHCOND'])
# Perform the Chi-square test
chi2, p, dof, expected = chi2_contingency(contingency_table)

print(f"Chi-square statistic: {chi2}")
print(f"P-value: {p}")
print(f"Degrees of freedom: {dof}")
print("Expected frequencies:\n", expected)


# Interpret the results
alpha = 0.05  # Significance level
if p < alpha:
    print("There is a statistically significant relationship between insurance type and health status")
else:
    print("There is no statistically significant relationship between insurance type and health status")

```

    Chi-square statistic: 3.322809309448495
    P-value: 0.6503514392781813
    Degrees of freedom: 5
    Expected frequencies:
     [[23.92929293 79.07070707]
     [20.21212121 66.78787879]
     [ 0.23232323  0.76767677]
     [ 0.46464646  1.53535354]
     [ 0.92929293  3.07070707]
     [ 0.23232323  0.76767677]]
    There is no statistically significant relationship between insurance type and health status



```python
#Chi-square between english proficiency and health status
# Create a contingency table
contingency_table = pd.crosstab(df['HWELLSPK'], df['HHCOND'])

# Perform the Chi-square test
chi2, p, dof, expected = chi2_contingency(contingency_table)

print(f"Chi-square statistic: {chi2}")
print(f"P-value: {p}")
print(f"Degrees of freedom: {dof}")
print("Expected frequencies:\n", expected)


# Interpret the results
alpha = 0.05  # Significance level
if p < alpha:
    print("There is a statistically significant relationship between english proficiency and health status")
else:
    print("There is no statistically significant relationship between english proficiency and health status")

```

    Chi-square statistic: 4.668008463489015
    P-value: 0.32308811918668695
    Degrees of freedom: 4
    Expected frequencies:
     [[25.09090909 82.90909091]
     [ 3.02020202  9.97979798]
     [ 0.46464646  1.53535354]
     [ 0.23232323  0.76767677]
     [17.19191919 56.80808081]]
    There is no statistically significant relationship between english proficiency and health status



```python
#Chi-square between race and health status
# Create a contingency table
contingency_table = pd.crosstab(df['RACEV1X'], df['HHCOND'])

# Perform the Chi-square test
chi2, p, dof, expected = chi2_contingency(contingency_table)

print(f"Chi-square statistic: {chi2}")
print(f"P-value: {p}")
print(f"Degrees of freedom: {dof}")
print("Expected frequencies:\n", expected)


# Interpret the results
alpha = 0.05  # Significance level
if p < alpha:
    print("There is a statistically significant relationship between race and health status")
else:
    print("There is no statistically significant relationship between race and health status")

```

    Chi-square statistic: 1.6768062814686604
    P-value: 0.642102541017975
    Degrees of freedom: 3
    Expected frequencies:
     [[ 37.86868687 125.13131313]
     [  3.94949495  13.05050505]
     [  1.39393939   4.60606061]
     [  2.78787879   9.21212121]]
    There is no statistically significant relationship between race and health status



```python
#Chi-square between diagnosis code and health status
# Create a contingency table
contingency_table = pd.crosstab(df['ICD10CDX'], df['HHCOND'])  # Example: Race vs. Health Status

# Perform the Chi-square test
chi2, p, dof, expected = chi2_contingency(contingency_table)

print(f"Chi-square statistic: {chi2}")
print(f"P-value: {p}")
print(f"Degrees of freedom: {dof}")
print("Expected frequencies:\n", expected)


# Interpret the results
alpha = 0.05  # Significance level
if p < alpha:
    print("There is a statistically significant relationship between diagnosis and health status")
else:
    print("There is no statistically significant relationship between diagnosis and health status")
```

    Chi-square statistic: 0.9898663104797409
    P-value: 0.6096116552792689
    Degrees of freedom: 2
    Expected frequencies:
     [[10.91919192 36.08080808]
     [24.62626263 81.37373737]
     [10.45454545 34.54545455]]
    There is no statistically significant relationship between diagnosis and health status

