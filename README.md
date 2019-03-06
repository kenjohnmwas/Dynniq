# Dynniq
Modelling Meetup attendance

For this modelling assignment, only the data from the events information was considered as the author assumed including other information from other data batches would be redundant.
For example, the city field or latitude and longitude fields from the groups data are highly corrrelated to the group_id field in the events data. The same case applies to inforamtion in the users and venues data represented in the events data by rsvps_user_id and venue_id, resepectively. 

The data was converted from json using Parsehub a service the author employs for webscraping. Additionally, the data was uploaded on Amazon Web services for easier manipulation between the tables using MySQL Queries and statements such as JOIN and COUNT but this was dropped when only the events data was chosen for modelling.

In the events data, the time field was converted to POSIXct time with time zone set to Amsterdam. Consequently, the day of week and hour were extracted for model building. The duration was converted to hours.

The following fields (description, created, rsvps_when, rsvp_limit, ) were dropped as the author considered them to have little effcet on the outcome of model or would bias the outcome in the case of (rsvps_guests) which has values in the event the rsvp is a "yes".

The modelling was done based on the Caret package in R language and the selected methods were Random forest and Classification Trees.
Further data manipulation included removing rows with NAs and random selection of rows to have a smaller dataset of 5000 rows.

The data with 5000 rows was first partitioned into Traindata for training the  model and Testdata for final model fitting.
The Traindata was further partitioned into Traindata1 and Traindata2 for model validation to asses the perfomance.

From the two models the random forests method had higher accuracy and was used to fit the Testdata.



