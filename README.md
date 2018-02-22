

```python
#dependencies
import pandas as pd

```


```python
# file path
file = "student/schools_complete.csv"
file1 = "student/students_complete.csv"
```


```python
#read csv 
school_df = pd.read_csv(file)
student_df =pd.read_csv(file1)

```


```python
#District Summary

Dsummary_table = pd.DataFrame([])

tot_school = school_df.loc[:, 'name'].count()
tot_budget = school_df.loc[:, 'budget'].sum()
tot_student = school_df.loc[:,'size'].sum()
ave_mathscore = student_df.loc[:,'math_score'].mean()
ave_readscore = student_df.loc[:, 'reading_score'].mean()

#calculating the mathpassing score  
c = 0
for i in student_df['math_score']:
    if (i >= 70):
        c= c+1
               
#print(c)        
per_mathpassing =  (c/tot_student)*100
#print(per_mathpassing)

#calculating the mathpassing score  
c = 0
for i in student_df['reading_score']:
    if (i >= 70):
        c= c+1
               
#print(c)        
per_readpassing =  (c/tot_student)*100
#print(per_readpassing)

#overall passing rate 
tot_passing = (per_mathpassing+ per_readpassing)/2
#print(tot_passing)
Dsummary_table = Dsummary_table.append(pd.DataFrame({'total School': tot_school, 'TotalBudget': tot_budget, 'Total Students': tot_student, 'Average Math Score': ave_mathscore, 'Average Reading Score': ave_readscore, 'per_mathpassing': per_mathpassing, 'per_readpassing': per_readpassing,'Overall Passing Rate': tot_passing,}, index=[0]), ignore_index=True)


Dsummary_table = Dsummary_table[['total School',  'Total Students', 'TotalBudget', 'Average Math Score', 'Average Reading Score', 'per_mathpassing', 'per_readpassing', 'Overall Passing Rate']]
Dsummary_table = Dsummary_table.rename(columns={'per_mathpassing':'%Passing Math', 'per_readpassing':'%Passing reading', 'Overall Passing Rate':'%Overall Passing Rate'})
#cols = list(Dsummary_table.columns.values)
#cols

Dsummary_table["TotalBudget"] = pd.to_numeric(Dsummary_table["TotalBudget"])
Dsummary_table["TotalBudget"] = Dsummary_table["TotalBudget"].map('${0:,.2f}'.format)
Dsummary_table
#.
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>total School</th>
      <th>Total Students</th>
      <th>TotalBudget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>%Passing Math</th>
      <th>%Passing reading</th>
      <th>%Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>15</td>
      <td>39170</td>
      <td>$24,649,428.00</td>
      <td>78.985371</td>
      <td>81.87784</td>
      <td>74.980853</td>
      <td>85.805463</td>
      <td>80.393158</td>
    </tr>
  </tbody>
</table>
</div>




```python
#school Summary

#summary_table = pd.DataFrame.from_records([{'schoolname': school_name, 'type_school': type_school, 'sudentcount': sudentcount, 'budgetsc': budgetsc, 'per_student': per_student, 'avg_mathscore': schoolave_mathscore, 'ave_readscore': schoolave_readscore,'% mathpassing': sper_mathpassing, '% readpassing': sper_readpassing, '%overall passing': schtot_passing}])
#creating dataframe
summary_table = pd.DataFrame([])

for index, row in school_df.iterrows():
    school_name = row["name"]
    type_school = row["type"]
    sudentcount = row["size"]
    budgetsc = row["budget"]
    per_student = budgetsc / sudentcount
    #print(str(school_name) + "   " + str(type_school) + " " + str(sudentcount) + "  " + str(budgetsc) + " " + str(per_student) )
    studentsubset_df = student_df.loc[student_df['school'] ==  school_name, ['school','reading_score','math_score']]
    schoolave_mathscore = studentsubset_df.loc[:,'math_score'].mean()
    schoolave_readscore = studentsubset_df.loc[:, 'reading_score'].mean()
    #print(str(schoolave_mathscore) + " "+ str(schoolave_readscore))
    #calculating the mathpassing score  
    c1 = 0
    for i in studentsubset_df['math_score']:
        if (i >= 70):
            c1= c1+1
    
    sper_mathpassing =  (c1/sudentcount)*100
    #print(sper_mathpassing)
    
    #calculating the readpassing score  
    c1 = 0
    for i in studentsubset_df['reading_score']:
        if (i >= 70):
            c1= c1+1
    
    sper_readpassing =  (c1/sudentcount)*100
    #print(sper_readpassing)
    
    #overall passing rate 
    schtot_passing = (sper_mathpassing+ sper_readpassing)/2
    #print(schtot_passing)
    #append data to summarytable
    #summary_table = summary_table.append(pd.DataFrame({'schoolname': school_name, 'type_school': type_school, 'sudentcount': sudentcount, 'budgetsc': budgetsc, 'per_student': per_student, 'avgmathscore': schoolave_mathscore, 'avgreadscore': schoolave_readscore,'% math passing': sper_mathpassing,}, index=[0]), ignore_index=True)
    summary_table = summary_table.append(pd.DataFrame({'schoolname': school_name, 'school Type': type_school, 'Total Students': sudentcount, 'Total School Budget': budgetsc, 'Per Student Budget': per_student, 'Average Math Score': schoolave_mathscore, 'Average Reading Score': schoolave_readscore,'Passing Math': sper_mathpassing, 'Passing reading': sper_readpassing, 'Overall passing Rate': schtot_passing,}, index=[0]), ignore_index=True)

#print(summary_table.tail())


#allign and print 
summary_table = summary_table[['schoolname',  'school Type', 'Total Students', 'Total School Budget','Per Student Budget', 'Average Math Score', 'Average Reading Score', 'Passing Math', 'Passing reading', 'Overall passing Rate']]
Re_summary_table = summary_table.rename(columns={'Passing Math':'%Passing Math', 'Passing reading':'%Passing reading', 'Overall passing Rate':'%Overall Passing Rate'})
#formating
Re_summary_table["Total School Budget"] = Re_summary_table["Total School Budget"].map('${0:,.2f}'.format)
Re_summary_table["Per Student Budget"] = Re_summary_table["Per Student Budget"].map('${0:,.2f}'.format)
Re_summary_table.set_index('schoolname', inplace=True)
Re_summary_table

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>school Type</th>
      <th>Total Students</th>
      <th>Total School Budget</th>
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>%Passing Math</th>
      <th>%Passing reading</th>
      <th>%Overall Passing Rate</th>
    </tr>
    <tr>
      <th>schoolname</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Huang High School</th>
      <td>District</td>
      <td>2917</td>
      <td>$1,910,635.00</td>
      <td>$655.00</td>
      <td>76.629414</td>
      <td>81.182722</td>
      <td>65.683922</td>
      <td>81.316421</td>
      <td>73.500171</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>District</td>
      <td>2949</td>
      <td>$1,884,411.00</td>
      <td>$639.00</td>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>65.988471</td>
      <td>80.739234</td>
      <td>73.363852</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>Charter</td>
      <td>1761</td>
      <td>$1,056,600.00</td>
      <td>$600.00</td>
      <td>83.359455</td>
      <td>83.725724</td>
      <td>93.867121</td>
      <td>95.854628</td>
      <td>94.860875</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>District</td>
      <td>4635</td>
      <td>$3,022,020.00</td>
      <td>$652.00</td>
      <td>77.289752</td>
      <td>80.934412</td>
      <td>66.752967</td>
      <td>80.862999</td>
      <td>73.807983</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>Charter</td>
      <td>1468</td>
      <td>$917,500.00</td>
      <td>$625.00</td>
      <td>83.351499</td>
      <td>83.816757</td>
      <td>93.392371</td>
      <td>97.138965</td>
      <td>95.265668</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>Charter</td>
      <td>2283</td>
      <td>$1,319,574.00</td>
      <td>$578.00</td>
      <td>83.274201</td>
      <td>83.989488</td>
      <td>93.867718</td>
      <td>96.539641</td>
      <td>95.203679</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>Charter</td>
      <td>1858</td>
      <td>$1,081,356.00</td>
      <td>$582.00</td>
      <td>83.061895</td>
      <td>83.975780</td>
      <td>94.133477</td>
      <td>97.039828</td>
      <td>95.586652</td>
    </tr>
    <tr>
      <th>Bailey High School</th>
      <td>District</td>
      <td>4976</td>
      <td>$3,124,928.00</td>
      <td>$628.00</td>
      <td>77.048432</td>
      <td>81.033963</td>
      <td>66.680064</td>
      <td>81.933280</td>
      <td>74.306672</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>Charter</td>
      <td>427</td>
      <td>$248,087.00</td>
      <td>$581.00</td>
      <td>83.803279</td>
      <td>83.814988</td>
      <td>92.505855</td>
      <td>96.252927</td>
      <td>94.379391</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>Charter</td>
      <td>962</td>
      <td>$585,858.00</td>
      <td>$609.00</td>
      <td>83.839917</td>
      <td>84.044699</td>
      <td>94.594595</td>
      <td>95.945946</td>
      <td>95.270270</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>Charter</td>
      <td>1800</td>
      <td>$1,049,400.00</td>
      <td>$583.00</td>
      <td>83.682222</td>
      <td>83.955000</td>
      <td>93.333333</td>
      <td>96.611111</td>
      <td>94.972222</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>District</td>
      <td>3999</td>
      <td>$2,547,363.00</td>
      <td>$637.00</td>
      <td>76.842711</td>
      <td>80.744686</td>
      <td>66.366592</td>
      <td>80.220055</td>
      <td>73.293323</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>District</td>
      <td>4761</td>
      <td>$3,094,650.00</td>
      <td>$650.00</td>
      <td>77.072464</td>
      <td>80.966394</td>
      <td>66.057551</td>
      <td>81.222432</td>
      <td>73.639992</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>District</td>
      <td>2739</td>
      <td>$1,763,916.00</td>
      <td>$644.00</td>
      <td>77.102592</td>
      <td>80.746258</td>
      <td>68.309602</td>
      <td>79.299014</td>
      <td>73.804308</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>Charter</td>
      <td>1635</td>
      <td>$1,043,130.00</td>
      <td>$638.00</td>
      <td>83.418349</td>
      <td>83.848930</td>
      <td>93.272171</td>
      <td>97.308869</td>
      <td>95.290520</td>
    </tr>
  </tbody>
</table>
</div>




```python
#displaying top Performing Schools (By Passing rate)

Re_summary_table.columns
Top_fiveschool = Re_summary_table.sort_values(by='%Overall Passing Rate',  ascending=False)
#formating
Top_fiveschool.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>school Type</th>
      <th>Total Students</th>
      <th>Total School Budget</th>
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>%Passing Math</th>
      <th>%Passing reading</th>
      <th>%Overall Passing Rate</th>
    </tr>
    <tr>
      <th>schoolname</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Cabrera High School</th>
      <td>Charter</td>
      <td>1858</td>
      <td>$1,081,356.00</td>
      <td>$582.00</td>
      <td>83.061895</td>
      <td>83.975780</td>
      <td>94.133477</td>
      <td>97.039828</td>
      <td>95.586652</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>Charter</td>
      <td>1635</td>
      <td>$1,043,130.00</td>
      <td>$638.00</td>
      <td>83.418349</td>
      <td>83.848930</td>
      <td>93.272171</td>
      <td>97.308869</td>
      <td>95.290520</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>Charter</td>
      <td>962</td>
      <td>$585,858.00</td>
      <td>$609.00</td>
      <td>83.839917</td>
      <td>84.044699</td>
      <td>94.594595</td>
      <td>95.945946</td>
      <td>95.270270</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>Charter</td>
      <td>1468</td>
      <td>$917,500.00</td>
      <td>$625.00</td>
      <td>83.351499</td>
      <td>83.816757</td>
      <td>93.392371</td>
      <td>97.138965</td>
      <td>95.265668</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>Charter</td>
      <td>2283</td>
      <td>$1,319,574.00</td>
      <td>$578.00</td>
      <td>83.274201</td>
      <td>83.989488</td>
      <td>93.867718</td>
      <td>96.539641</td>
      <td>95.203679</td>
    </tr>
  </tbody>
</table>
</div>




```python
#displaying Bottom Performing Schools (By Passing rate)

Re_summary_table.columns
bottom_fiveschool = Re_summary_table.sort_values(by='%Overall Passing Rate',  ascending=False)
bottom_fiveschool.tail()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>school Type</th>
      <th>Total Students</th>
      <th>Total School Budget</th>
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>%Passing Math</th>
      <th>%Passing reading</th>
      <th>%Overall Passing Rate</th>
    </tr>
    <tr>
      <th>schoolname</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Ford High School</th>
      <td>District</td>
      <td>2739</td>
      <td>$1,763,916.00</td>
      <td>$644.00</td>
      <td>77.102592</td>
      <td>80.746258</td>
      <td>68.309602</td>
      <td>79.299014</td>
      <td>73.804308</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>District</td>
      <td>4761</td>
      <td>$3,094,650.00</td>
      <td>$650.00</td>
      <td>77.072464</td>
      <td>80.966394</td>
      <td>66.057551</td>
      <td>81.222432</td>
      <td>73.639992</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>District</td>
      <td>2917</td>
      <td>$1,910,635.00</td>
      <td>$655.00</td>
      <td>76.629414</td>
      <td>81.182722</td>
      <td>65.683922</td>
      <td>81.316421</td>
      <td>73.500171</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>District</td>
      <td>2949</td>
      <td>$1,884,411.00</td>
      <td>$639.00</td>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>65.988471</td>
      <td>80.739234</td>
      <td>73.363852</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>District</td>
      <td>3999</td>
      <td>$2,547,363.00</td>
      <td>$637.00</td>
      <td>76.842711</td>
      <td>80.744686</td>
      <td>66.366592</td>
      <td>80.220055</td>
      <td>73.293323</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Math Scores by Grade

school_group = student_df.groupby(['school','grade'], as_index = False)['math_score'].agg('mean')

new_df = school_group.pivot(index='school', columns='grade')['math_score']

new_df = new_df[['9th',  '10th', '11th', '12th']]
new_df

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>grade</th>
      <th>9th</th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>77.083676</td>
      <td>76.996772</td>
      <td>77.515588</td>
      <td>76.492218</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>83.094697</td>
      <td>83.154506</td>
      <td>82.765560</td>
      <td>83.277487</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>76.403037</td>
      <td>76.539974</td>
      <td>76.884344</td>
      <td>77.151369</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>77.361345</td>
      <td>77.672316</td>
      <td>76.918058</td>
      <td>76.179963</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>82.044010</td>
      <td>84.229064</td>
      <td>83.842105</td>
      <td>83.356164</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>77.438495</td>
      <td>77.337408</td>
      <td>77.136029</td>
      <td>77.186567</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.787402</td>
      <td>83.429825</td>
      <td>85.000000</td>
      <td>82.855422</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>77.027251</td>
      <td>75.908735</td>
      <td>76.446602</td>
      <td>77.225641</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>77.187857</td>
      <td>76.691117</td>
      <td>77.491653</td>
      <td>76.863248</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>83.625455</td>
      <td>83.372000</td>
      <td>84.328125</td>
      <td>84.121547</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>76.859966</td>
      <td>76.612500</td>
      <td>76.395626</td>
      <td>77.690748</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>83.420755</td>
      <td>82.917411</td>
      <td>83.383495</td>
      <td>83.778976</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>83.590022</td>
      <td>83.087886</td>
      <td>83.498795</td>
      <td>83.497041</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>83.085578</td>
      <td>83.724422</td>
      <td>83.195326</td>
      <td>83.035794</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>83.264706</td>
      <td>84.010288</td>
      <td>83.836782</td>
      <td>83.644986</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Reading Scores by Grade

Read_group = student_df.groupby(['school','grade'], as_index = False)['reading_score'].agg('mean')

newread_df = Read_group.pivot(index='school', columns='grade')['reading_score']

newread_df = newread_df[['9th',  '10th', '11th', '12th']]
newread_df

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>grade</th>
      <th>9th</th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>81.303155</td>
      <td>80.907183</td>
      <td>80.945643</td>
      <td>80.912451</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>83.676136</td>
      <td>84.253219</td>
      <td>83.788382</td>
      <td>84.287958</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>81.198598</td>
      <td>81.408912</td>
      <td>80.640339</td>
      <td>81.384863</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>80.632653</td>
      <td>81.262712</td>
      <td>80.403642</td>
      <td>80.662338</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>83.369193</td>
      <td>83.706897</td>
      <td>84.288089</td>
      <td>84.013699</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>80.866860</td>
      <td>80.660147</td>
      <td>81.396140</td>
      <td>80.857143</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.677165</td>
      <td>83.324561</td>
      <td>83.815534</td>
      <td>84.698795</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>81.290284</td>
      <td>81.512386</td>
      <td>81.417476</td>
      <td>80.305983</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>81.260714</td>
      <td>80.773431</td>
      <td>80.616027</td>
      <td>81.227564</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>83.807273</td>
      <td>83.612000</td>
      <td>84.335938</td>
      <td>84.591160</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>80.993127</td>
      <td>80.629808</td>
      <td>80.864811</td>
      <td>80.376426</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>84.122642</td>
      <td>83.441964</td>
      <td>84.373786</td>
      <td>82.781671</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>83.728850</td>
      <td>84.254157</td>
      <td>83.585542</td>
      <td>83.831361</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>83.939778</td>
      <td>84.021452</td>
      <td>83.764608</td>
      <td>84.317673</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>83.833333</td>
      <td>83.812757</td>
      <td>84.156322</td>
      <td>84.073171</td>
    </tr>
  </tbody>
</table>
</div>



#sample bin test example
#school_group.pivot(index='school', columns='grade', values = 'math_score')
#import numpy as np
#table = pd.pivot_table(school_group, values='math_score', index=['school'], columns=['grade'], aggfunc=np.sum)
#table
#newread_df.columns
#bin
bins = [70, 80, 90, 95, 100]
labels = [1,2,3,4]
newread_df['binned'] = pd.cut(newread_df['9th'], bins=bins, labels=labels)
newread_df.head(3)


```python
#Scores by School Spending

#copying one dataframe toanother and renaming the columns
Scoresschooldf = summary_table.copy()
Scoresschooldf = Scoresschooldf.rename(columns={'Passing Math':'%Passing Math', 'Passing reading':'%Passing reading', 'Overall passing Rate':'%Overall Passing Rate'})

#Bins and lables
bins = [575, 585, 615, 645, 675]
labels = ['<585', '$585-615','$615-645','$645-675']

#based on perstudent budget 
pd.cut(Scoresschooldf['Per Student Budget'], bins=bins, labels=labels)
Scoresschooldf['binned'] = pd.cut(Scoresschooldf['Per Student Budget'], bins=bins, labels=labels)

Scores_bindf = Scoresschooldf[['binned', 'Average Math Score', 'Average Reading Score', '%Passing Math', '%Passing reading', '%Overall Passing Rate']]
Scores_bindf

Scores_group = Scores_bindf.groupby("binned")
Scores_group.max()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>%Passing Math</th>
      <th>%Passing reading</th>
      <th>%Overall Passing Rate</th>
    </tr>
    <tr>
      <th>binned</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;585</th>
      <td>83.803279</td>
      <td>83.989488</td>
      <td>94.133477</td>
      <td>97.039828</td>
      <td>95.586652</td>
    </tr>
    <tr>
      <th>$585-615</th>
      <td>83.839917</td>
      <td>84.044699</td>
      <td>94.594595</td>
      <td>95.945946</td>
      <td>95.270270</td>
    </tr>
    <tr>
      <th>$615-645</th>
      <td>83.418349</td>
      <td>83.848930</td>
      <td>93.392371</td>
      <td>97.308869</td>
      <td>95.290520</td>
    </tr>
    <tr>
      <th>$645-675</th>
      <td>77.289752</td>
      <td>81.182722</td>
      <td>66.752967</td>
      <td>81.316421</td>
      <td>73.807983</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Score by School Size

#copying dataframe toanother and renaming the columns
Schoolsizedf = summary_table.copy()
Schoolsizedf = Schoolsizedf.rename(columns={'Passing Math':'%Passing Math', 'Passing reading':'%Passing reading', 'Overall passing Rate':'%Overall Passing Rate'})


bins = [426, 1000, 2000, 5000]
labels = ['Small(<1000)', 'Medium(1000-2000)', 'Large(2000-5000)']

pd.cut(Schoolsizedf['Total Students'], bins=bins, labels=labels)
Schoolsizedf['binned'] = pd.cut(Schoolsizedf['Total Students'], bins=bins, labels=labels)

Scores_schooldf = Schoolsizedf[['binned', 'Average Math Score', 'Average Reading Score', '%Passing Math', '%Passing reading', '%Overall Passing Rate']]
Scores_schoolgroup = Scores_schooldf.groupby("binned")
Scores_schoolgroup.max()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>%Passing Math</th>
      <th>%Passing reading</th>
      <th>%Overall Passing Rate</th>
    </tr>
    <tr>
      <th>binned</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Small(&lt;1000)</th>
      <td>83.839917</td>
      <td>84.044699</td>
      <td>94.594595</td>
      <td>96.252927</td>
      <td>95.270270</td>
    </tr>
    <tr>
      <th>Medium(1000-2000)</th>
      <td>83.682222</td>
      <td>83.975780</td>
      <td>94.133477</td>
      <td>97.308869</td>
      <td>95.586652</td>
    </tr>
    <tr>
      <th>Large(2000-5000)</th>
      <td>83.274201</td>
      <td>83.989488</td>
      <td>93.867718</td>
      <td>96.539641</td>
      <td>95.203679</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Scores by School Type 

#copying dataframe toanother and renaming the columns
Schooltype_df = summary_table.copy()
Schooltype_df = Schooltype_df.rename(columns={'Passing Math':'%Passing Math', 'Passing reading':'%Passing reading', 'Overall passing Rate':'%Overall Passing Rate'})

school_group1 = Schooltype_df.groupby(['school Type'], as_index = False)['Average Math Score', 'Average Reading Score','%Passing Math','%Passing reading', '%Overall Passing Rate'].agg('mean')
school_group1.set_index('school Type', inplace=True)

#display
school_group1


```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>%Passing Math</th>
      <th>%Passing reading</th>
      <th>%Overall Passing Rate</th>
    </tr>
    <tr>
      <th>school Type</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Charter</th>
      <td>83.473852</td>
      <td>83.896421</td>
      <td>93.620830</td>
      <td>96.586489</td>
      <td>95.103660</td>
    </tr>
    <tr>
      <th>District</th>
      <td>76.956733</td>
      <td>80.966636</td>
      <td>66.548453</td>
      <td>80.799062</td>
      <td>73.673757</td>
    </tr>
  </tbody>
</table>
</div>



#Three observable trends I came up with are
1) Charter school have better passing rate then District Schools. 
2) Number of students does not reflect for the over all passing rate. For Example there are less students in Holden High School, But they have the same passing rate as Wilson High School which has more students.
3) School Budget refelects the average math scores 

