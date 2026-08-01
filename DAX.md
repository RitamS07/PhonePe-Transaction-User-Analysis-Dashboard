**---FOR CREATING ANOTHER TABLE FOR FURTHER TIME CLARIFICATION**

Date\_Table = ADDCOLUMNS(

&#x20;   CALENDAR(MIN(All\_Transactions\[Date]), MAX(All\_Transactions\[Date])),

&#x20;   "Year", YEAR(\[Date]),

&#x20;   "Month No.", MONTH(\[Date]),

&#x20;   "Month", FORMAT(\[Date],"MMM"),

&#x20;   "Quater","Q" \& FORMAT(\[Date],"Q"),

&#x20;   "Weekday", FORMAT(\[Date],"dddd"),

&#x20;   "Day No.", WEEKDAY(\[Date],2),

&#x20;   "Weekend", IF(WEEKDAY(\[Date],2)>=6,"Weekend","Weekday")

)







**---Connect used\_id of All\_users to All\_transaction  (one to many)    \& Date of All\_transaction to Date\_Table   (One to many)**





**---Create New Measures for**

* Trans Values PM = CALCULATE(\[Total  Transaction Values],DATEADD(Date\_Table\[Date],-1,MONTH))
* Trans Value MOM% = DIVIDE(\[Total  Transaction Values]-\[Trans Values PM],\[Trans Values PM],0)
* Total Users = DISTINCTCOUNT(All\_Users\[User\_ID])
* Total Transaction = COUNT(All\_Transactions\[Transaction\_ID])
* Total  Transaction Values = SUM(All\_Transactions\[Amount])
* Successful Transaction =CALCULATE(

&#x20;   COUNT(All\_Transactions\[Transaction\_ID]),

&#x20;   CONTAINSSTRING(All\_Transactions\[Payment\_Status], "Success")

&#x09;)

* Total Trans PM =

&#x09;CALCULATE(

&#x20;  	 \[Total Transaction],

&#x20;  	 DATEADD(Date\_Table\[Date], -1, MONTH)

&#x09;)

* Total Trans MOM% =

&#x09;DIVIDE(

&#x20;   	\[Total Transaction] - \[Total Trans PM],

&#x20;   	\[Total Trans PM],

&#x20;  	 0

&#x09;)

* Success Rate = DIVIDE(\[Successful Transaction],\[Total Transaction])





