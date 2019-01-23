# Call center modeling

The purpose of this project is two-fold
1. Efficiently simulate a random day for a generica call-center, then record statistics for analysis
2. Track the performance of different code configurations

Potential business use cases for this analysis:
* Determine optimal configuration (number of representatives) given response-time KPIs 
* Identify potential bottle-neck conditions not present in historic data
* Provide a method to allow minute-to-minute assessment of letting representatives go on break

This project was done in colaboration, particulary the Roland_queue approach.

# General Process flow:

Customers come into the system at a random rate throughout the day (based on historic data). 
At this point, they temporarily enter a queue with lowest priority (if it exists). 
IF a representatvive is available, THEN they are paired with the customer with the highest priority. 
The customer is no longer in the queue, and the representative
is not available for the remaining duration of the call (a random number based on historic data). 

ELSE, the customer is remains in a queue and is given the highest available priority in the existing queue. 

# Approaches

For all of these approaches, a default configuration was applied to determine the time of the 
calculation (and efficiency of the code). 

## Nested For-loops (process_flow_modeling, call_center_simulation,call_center_simulation_v2). 
This method explicitly loops through each time increment. It checks for new customers, then 
blocks off time for the <i>highest priority representative</i>. The information is explicitly written to a 
data frame. Time to process default configuration: 4 minutes, 30 seconds

## Single integer operation (min_funct_)
This method relies on calculting a single number: the exit time for a customer. This
number is used to infer all other customer metrics. In this formulation, customers are assigned to the <i>next available</i>
representative. Additionally, the stored data is only re-calculated as new customers arrive, 
dramatically reducing the number of calculations.
Once the 'day' is completed, the metrics are unpacked into a dataframe for more digestible
content. Time to process default configuration: 45 miliseconds. 

## True Simulation / Threading (Roland_queue)
Building on a completely different approach, this method pairs off customers with representatives
each in a queue, then spins the pair into a separate thread. Each of these threads then waits to see
if it has completed before re-inserting the representative in the queue. 

Currently, the results are not recorded but rather shown. 

# Background 
Reading on typical use cases and workflows can be found here. 
* https://www.callcentrehelper.com/search-results.php?q=call+routing+strategies
* https://www.twilio.com/learn/contact-center/taskrouter
* https://help.genesys.com/cic/mergedprojects/wh_tr/desktop/pdfs/acd_processing_tr.pdf
