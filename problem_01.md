## Problem 01

Consider the following table:


```
CREATE TABLE billing_history (
  id INT NOT NULL AUTO_INCREMENT,
  subscription_id INT NOT NULL,
  bill_date DATE DEFAULT NULL,
  is_billed BOOL DEFAULT FALSE,
  PRIMARY KEY (`id`)
);
```

Find pair of subscription_ids and their last successfull bill's date, limited to subscriptions that their billing has been failed within last seven days, and filter the results to subscription_ids that their last successfull bill date lies before past three months. This covers subscriptions that were billable more than three months ago, but in the last seven days were not billed.

## Solution
```
SELECT
	subscription_id,
	DATE(MAX(CASE WHEN is_billed = TRUE THEN bill_date ELSE NULL END)) last_success_billed
FROM
	billing_history
JOIN
	(
	SELECT
		DISTINCT s.subscription_id
	FROM
		billing_history s
	WHERE
		s.bill_date >= DATE_SUB(CURDATE(), INTERVAL 7 DAY) AND
		s.is_billed = FALSE
	) f
USING
	(subscription_id)
GROUP BY
	subscription_id
HAVING
	last_success_billed < DATE_SUB(CURDATE(), INTERVAL 3 MONTH)
```
