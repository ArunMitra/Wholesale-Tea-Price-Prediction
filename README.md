# Wholesale-Tea-Price-Prediction
Wholesale tea auction prices prediction

## Prediciting wholesale tea auction prices at Tea Board of India Auctions - a work-in-progress

### Background

This project came to my attention through a friend who works as a consultant for a tea auctioneering company based in Cochin in Southern India. He asked whether I could help to build models to predict auction prices at weekly tea auctions.

Tea is a huge market around the world, and India is a leading producer. About 1.4 million kgs of tea were produced in India in 2019.
Considering the complex logistics involved before the tea reaches the consumer, good predictive models would be very useful for all parties involved in the tea auction trade for some of the following reasons:

- **For Sellers (Tea Estates):** 
    The question is what quantity to put up for any given weekly auction at any one of 6 auction centers around the country run by the Tea Board of India. 
    The tea must be despatched to the auction center **2 to 3 weeks prior to the date of the auction.** 
    Typically, there are 2 or 3 auction centers that are within reach of trucks within that time span and the seller must decide where to send how much.

- **For Buyers (Tea Packeteers):**
    Big packeteers store upto 2 to 2 and a half months of stock at any given point in time, and hence must plan their buying & blending ahead of time.
    Exporters take a lot of time to fulfil their large orders and have to quote prices in advance to importers.
    
- **For Auctioneers:**
   Auctioneers need give market feedback and intelligence reports to buyers and sellers. My friend believes that the quality of the reports that he would be able
   to offer would be greatly enhanced with good predictive analytics. Currently, he said, all they offer is based on intuition! See the last page of the attached 
   report.

The **4 major types** of teagrown in India are:
- **CTC** (Crush, tear, curl: ensures a faster production of a standard quality)
- **Orthodox** (focuses on preserving the singular virtues of the leaf resulting in fermented tea leaves)

THe above are further divided into"
- **Leaf** (the better quality leaves and curl), and
- **Dust** (the dust and fannings leftover from broken tea leaves, so in essence - the waste)

Tea buyers and sellers always have a rolling stock of tea at any point in time. Even though tea is perishable it is stored upto 9 months. Both buyers and sellers accumulate stocks over time as the market dictates(demand/supply shocks). 
Tea supply and tea quality are both very unpredictable and are heavily dependent on regional weather patterns. 

As far as is currently known, Hindustan Unilever Limited (as a tea packeteer) is the only company in India that is performing sophisticated predictive analytics internally, using not only historical data about:
- offer quantities
- sold quantities
- average prices

but also has incorporated other features like:
- quality indices
- market feedback on quality
- ... and more...

## The Goals of this Project:

- To build a continuosly updated predictive analytics pipleline that can be useful to all players in the market, to bring about efficiencies
- This will be a non-profit project, interested parties will be able to access model predictions at a nominal fee to cover costs   
- This repo represents the initial exploratory work that has been done towards this goal

## The Process

- Data: Get and clean data
- EDA: Explore and analyze the data
- Score: Set a metric to evaluate the models
- Base model: Iterate through various model options to find the best base model
- Tune Model: Search for the best hyperparameters
- Start with a limited scope and iteratively expand the features that will be used
    - Limit the types of tea to the 4 majors, then incorporate the other categories
    - Limit the auction centers, start with 1 in S, India, then include the others in S. India. THen move to N. India centers (where the patterns are different)
    - Start with univariate time series models (price only), then move to multivariate models with quantity features added, then incorporate multiple categories,
      then include multiple bnear-located auction centers (S. India, N. India separately)

## The Data

- Data Source
  - [Tea Board of India](https://www.teaauction.gov.in/pages/News.aspx)
  
- Description of Data: 
  - **Weekly data from 2012 until the present on the following features:**
    - Offered Quantities
    - Sold Qualtities
    - Average Price
    
    for ...
    
    **10 types of tea**
    - CTC Leaf
    - CTC Dust
    - Ortho Leaf
    - Ortho Dust
    - Kangra Leaf
    - Kangra Dust
    - Darj Leaf
    - Darj Dust
    - Green Leaf
    - Green Dust
    
    **across 7 auction centers in India**
    - Cochin
    - Coimbatore
    - Coonoor
        ... in Southern India, and
    - Kolkata
    - Guwahati
    - Jalpaiguri
    - Siliguri
        ... in Northern India
    
    This repo is currently (22 September 2020) limited to the work on:
    - Data starting from 2012 (prior years' data had different aggregations of tea types)
    - The 4 major tea types only (CTC Leaf, CTC Dust, Ortho Leaf, Ortho Dust)
    - Only 1 South Indian auction center (Cochin)
    - North Indian auction centers were left aside for now. NOTE: North Indian tea supply has a very different seasonality from South Indian tea supply due to the 
      fact that South Indian tea grows in an equatorial climate and is plucked all year round

- Data Cleaning
  By checking for outliers and missing data, it was found that:
    - Some of the data (especially form the early years) appears to have involved manual data entry as there were obviuos human errors in the data (for example, 
      2 consecutive cells on the the samae row had the same values even though one was quantity in Kgs - in the order of 10s of thousands, and the other was price
      per Kg - in the order of INR 70 to INR 110)
    - A small percentage (<2%) of NaNs and Zero values were present
      These were substituted with moving averages from the the past 3 observations(weeks)
    
## EDA

- Overall available data points = 448
- Quantities offered, sold and average price for the Cochin Auction Center from 2012 to 2020 were as follows:

  CTC Leaf:
 [CTC Leaf Data][Images/CTC_LEAF_Cochin_oqty_sqty_avgp2020_09_21_04_50_55.png]



- churn percentage : 20.37%
- A balanced, well-crafted sample
- Bar charts, histograms and scatter matrix plots of various aspects of the data are to be fould in the jupyter notebook in the folder /Notebook
- No obvious correlations between features was observed
- Looking at churn vs. individual features, customer age, balance and number of products a customer uses had some clear differences in distributtion between churners and non-churners. All all other features no difference in distribution was observed 

## Model Scoring

Recall: is the metric we are most interested in because false negatives (not finding churners) is what we most want to minimize
Precision: is also interesting
Accuracy: should not be too low

## Base Model

- Started with 3 options:
  - Logistic Regression
  - Random Forest Classifier
  - Gradient Boosting Classifier
- First results:
  - Recall Scores were poor for all the models:
  - Then balanced the classes using:
    - Undersampling of the majority class
    - Random oversampling of the minority class (with replacement)
    - Synthetic Minority Class Oversampling Technique(SMOTE)
    - A combination of Undersampling and SMOTE
    
    Detail results for each of these techniques for all three models are to be found in the jupyter notebook in the /Notebook folder in this repo, and are summarized in the slide presentation in the /Presentation folder in this repo
- Following the above, the GradientBoosting Model with random minority class oversampling was chosen as the base model (based on consistently best Recall scores)
- A check on whether the model would improve after dropping least important features based on feature importances was done, but did not improve scores

## Tuning the base model to find the best model

- Several rounds of GridSearch with various parametrs (detailed in the jupyter notebook to be found in the folder /Notebook in this repo) were done but Recall score could not be improved)
  - Results for GradientBoostingClassifier
    - Gridsearched best model Recall: 0.709
    - Base model Recall: 0.748
- The base model was thus adopted as the best model to proceed with

## Profit Curve

Assumptions:

- A customer who leaves the bank will result in average annual revenue loss of $1,000 ... this was based on:
  - Retail Lending Interest Rate: 1.80% (Source: European Central Bank 2019)
  - Retail Deposit Interest Rate: 0.37% (Source: European Central Bank 2019)
  - Average balance/customer (in sample): $76,485
  - Average earning/customer (in sample): $76,486 * (0.18 - 0.037) =  $1,094
  - Cross-validated with Annual Report of JP Morgan Chase from 2011 which had = $912 (Source: http://investor.shareholder.com/jpmorganchase/annual.cfm)

- $200 per likely-to-churn-customer, which could pay for:
  - Fee reductions
  - Better savings interest rates
  - A birthday credit from the bank
  - Individual outreach by customer service or email
  - Glossy brochure or other token gifts
  - Etc.

- Retention Program is 100% effective

With the assumptions for the cost benefit matrix, and the calculated confusion matrix, the profit curve was plotted. This is to be found in the jupyter notebook in the folder /Notebooks in this repo
The findings were that, with a $200 per customer retention program
- The maximum incremental profit would be $68,400
- At a conservative churn probability threhold of 0.25

## Next Steps
- Try Neural Network models to see if Recall can be improved
- All customers are not the same, consider more retention program spending on High-Value vs Low-Value customers
- Use Uplift Models to skew the retention program spend towards customers that are predicted to be more persuadable
- Explore the effects on the Profit Curve for various levels of retention program costs
