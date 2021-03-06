# New-Customer-Summary
<query>
		<title>New Customer Summary</title>
		<permissions>MARKETING</permissions>
		<sql><![CDATA[
			SELECT soi.store_id, GREATEST(SUM(soi.qty_ordered), SUM(soi.qty_shipped), SUM(soi.qty_invoiced)) - SUM(soi.qty_canceled) - SUM(soi.qty_refunded) - SUM(soi.qty_returned) AS 'total_actual_qty', 
				SUM(soi.price * (GREATEST(soi.qty_ordered, soi.qty_shipped, soi.qty_invoiced) - soi.qty_canceled - soi.qty_refunded - soi.qty_returned)) AS 'total_revenue', 
			    COUNT(DISTINCT soi.order_id) AS 'number_of_orders', COUNT(DISTINCT so.customer_email) AS 'number_of_customers'
			FROM sales_order so JOIN sales_order_item soi ON so.entity_id = soi.order_id JOIN customer_entity ce ON ce.email = so.customer_email AND ce.store_id = so.store_id
			WHERE soi.price > 0 AND (SELECT COUNT(*) FROM customer_entity ce WHERE DATE(ce.created_at) >= ? AND DATE(ce.created_at) <= ? AND ce.entity_id = so.customer_id) > 0 AND DATE(soi.created_at) >= ? AND DATE(soi.created_at) <= ? 
			GROUP BY soi.store_id;
		]]></sql>
		<field id="0" type="date">
			<label>From</label>
			<dateformat/>
		</field>
		<field id="1" type="date">
			<label>To</label>
			<dateformat/>
		</field>
		<field id="2" type="date">
		<label>From</label>
			<dateformat/>
		</field>
		<field id="3" type="date">
			<label>To</label>
			<dateformat/>
		</field>
		<filename> New Customer Summary <date id="0"/> and <date id="1"/></filename>
	</query>
