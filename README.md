# Uber_ride_analysis


Uber Ride Data Analysis 🚕 Introduction

Ride-sharing platforms generate large volumes of operational data that can be used to understand user behavior, service efficiency, and booking outcomes. Analyzing this data helps identify patterns related to ride demand, cancellations, payment preferences, and overall platform performance.

In this notebook, we perform an Exploratory Data Analysis (EDA) on an Uber ride bookings dataset. The goal is to clean the data, investigate important variables, and uncover insights about how ride bookings behave across different scenarios.

The analysis focuses on several key questions:

What are the most common ride outcomes (completed, cancelled, incomplete)?

What are the main reasons behind ride cancellations?

Which payment methods are most frequently used?

Are there noticeable patterns in ride bookings that could indicate operational issues?

By the end of this Analysis , we will uncover the most important insights required to answer the questions.

--Cleaning Data and Creating a ROUTE(column) by merging pickup_location and drop_location

















```
 SELECT *
FROM UBER_BOOKINGS
FETCH FIRST 10 ROWS ONLY;



SELECT SUM(BOOKING_VALUE_NEW) AS TOTAL_REVENUE
FROM UBER_BOOKINGS;

-- RIDES PER MONTH

SELECT 
    TO_CHAR(DATEE, 'YYYY-MM') AS MONTH,
    COUNT(*) AS TOTAL_RIDES,
    ROUND((BOOKING_VALUE_NEW),2) AS avg_fare
FROM UBER_BOOKINGS
GROUP BY TO_CHAR(DATEE, 'YYYY-MM')
ORDER BY MONTH;
--performance by vehicletype

SELECT 
    VEHICLE_TYPE,
    COUNT(*) AS TOTAL_RIDES,
    ROUND(AVG(RIDE_DISTANCE),2) AS AVG_DISTANCE_KM,
    ROUND(AVG(BOOKING_VALUE),2) AS AVG_FARE,
    ROUND(SUM(BOOKING_VALUE),2) AS TOTAL_REVENUE,
    ROUND(AVG(DRIVER_RATINGS),2) AS AVG_DRIVER_RATING
FROM UBER_BOOKINGS
WHERE BOOKING_STATUS = 'Completed'
GROUP BY vehicle_type
order by total_revenue desc;

--Payment method distribution 

SELECT
    PAYMENT_METHOD,
    COUNT(*) AS TOTAL_RIDES,
    ROUND(SUM(BOOKING_VALUE),2) AS TOTAL_REVENUE,
    ROUND(COUNT(*) * 100 / SUM(COUNT(*)) OVER(),2) AS PERCENTAGE
FROM UBER_BOOKINGS
WHERE BOOKING_STATUS = 'Completed'
GROUP BY PAYMENT_METHOD
ORDER BY total_rides desc;

--avg fare & distance by vehicle

SELECT 
    VEHICLE_TYPE,
    ROUND(AVG(BOOKING_VALUE),2) AS AVG_FARE,
    ROUND(AVG(RIDE_DISTANCE),2) AS AVG_DISTANCE,
    ROUND(AVG(BOOKING_VALUE)/ NULLIF(AVG(RIDE_DISTANCE),0),2) AS FARE_PER_KM
FROM UBER_BOOKINGS
WHERE BOOKING_STATUS = 'Completed'
GROUP BY VEHICLE_TYPE
ORDER BY FARE_PER_KM DESC;```




```--PERCENTAGE OF  RESASON FOR WHICH  CANCELLED RIDES BY CUSTOMERS
SELECT 
    REASON_FOR_CANCELLING_BY_CUSTOMER,
    COUNT(*) as RIDES_CANCELLED,
    ROUND(COUNT(*)*100/ SUM(COUNT(*)) OVER(), 2) AS PERCENTAGE
FROM 
    UBER_BOOKINGS
WHERE
    CANCELLED_RIDES_BY_CUSTOMER = '1' AND
    reason_for_cancelling_by_customer is not null
GROUP BY 
    reason_for_cancelling_by_customer;
    
--PERCENTAGE OF REASONS CANCELLED BY DRIVER    
SELECT 
    DRIVER_CANCELLATION_REASON,
    COUNT(*) AS RIDES_CANCELLED,
    ROUND(COUNT(*)*100/SUM(COUNT(*)) OVER(), 2) AS PERCENTAGE
FROM UBER_BOOKINGS
WHERE 
    CANCELLED_RIDES_BY_DRIVER = '1' AND
    driver_cancellation_reason IS NOT NULL
GROUP BY driver_cancellation_reason
ORDER BY RIDES_CANCELLED; ```
    



