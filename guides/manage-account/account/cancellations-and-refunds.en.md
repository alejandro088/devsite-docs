---
  indexable: false
---

# Refunds and cancellations

There are different situations in which you may want to cancel a sale:

* If the payment status is `pending` or `in_process`, it means that the buyer has not been charged yet, so you can make a cancellation.

* If the payment `status` is `approved`, it means that the buyer was charged, so you can make a refund.

> WARNING
>
> WARNING
>
> Note that for payments with QR and POINT, you can only make refunds but not cancellations.

## Cancellations

- Cancellations can be made only with pending and in process transactions
- It is important for offline payment methods
- The expiration of a payment occurs after 30 days and the cancellation is automatic, is this case the final status of the payment is cancelled/expired

Only` pending` or `in_process` payments can be cancelled. As soon as you cancel them, they will no longer be approved and you will be able to release the stock pending confirmation.

Cancellations are mainly used with **cash payments**.

These payments do not expire automatically, so you need to cancel them.

To cancel, make the following request by submitting the `status` `cancelled`:

[[[
```php
<?php

  $payment = MercadoPago\Payment::find_by_id($payment_id);
  $payment->status = "cancelled";
  $payment->update();

?>
```
```java
Payment payment = Payment.findById(paymentId);
payment.setStatus("cancelled");
payment.update();

```
```node

mercadopago.payment.update({
  id: paymentId,
  status: "cancelled"
}).then().catch();

```
```ruby
preapproval = MercadoPago::Payment.find_by_id(paymentId)
preapproval.status = "cancelled"
preapproval.update()
```
```curl
curl -X PUT \
-H "Content-Type: application/json" \
-H 'Authorization: Bearer ACCESS_TOKEN' \
-d '{"status":"cancelled"}' \
'https://api.mercadopago.com/v1/payments/:ID'
```
]]]

**Response status code: 200 OK**

## Refunds
----[mla]----
You can refund a payment within **360 days** after it was approved.
------------
----[mlb]----
You can refund a payment within **180 days** after it was approved.
------------
----[mlm]----
You can refund a payment within **180 days** after it was approved.
------------
----[mlc]----
You can refund a payment within **330 days** after it was approved.
------------
----[mlu, mpe]----
You can refund a payment within **90 days** after it was approved.
------------
----[mco]----
You can refund a payment within **180 days** after it was approved.
------------

You must have sufficient funds in your account in order to successfully refund the payment amount. Otherwise, you will get a `400 Bad Request error`.

If the buyer made the payment with card, the amount will be refunded directly to the card.

If the payment was made by other means, the amount will be deposited in the buyer’s Mercado Pago account. If the buyer has no account, we will create one using the buyer’s e-mail address.

### Issue a full refund

To issue a full refund, make the following request indicating the `payment_id`:

```php
<?php

$payment = MercadoPago\Payment::find_by_id($payment_id);
$payment->refund();

?>
```
```node
mercadopago.payment.refund(payment_id)
  .then(function (response) {
    //Process response...
  })
  .catch(function (error) {
    //Handle the error ...
  });
```
```curl
curl -X POST \
-H "Content-Type: application/json" \
-H 'Authorization: Bearer ACCESS_TOKEN' \
'https://api.mercadopago.com/v1/payments/:ID/refunds'
```

> NOTE
>
> Nota
>
> El pago quedará con `status` en `refunded`.


**Response status code: 201 Created**

```json
{
	"id": REFUND_ID,
	"payment_id": ID,
	"amount": 73.48,
	"metadata": {},
	"source": {
		"id": "130379930",
		"name": "Firstname Lastname",
		"type": "collector"
	},
	"date_created": "2014-12-11T11:26:40.537-04:00"
}
```

### Issue a partial refund

You can issue up to 20 partial refunds for one single payment. Upon completion, the payment `status` will be `approved` with a `status_detail` in `partially_refunded`.

You must indicate the amount to be refunded.

[[[

```php
<?php
  $payment = MercadoPago\Payment::find_by_id(paymentId);
  $payment->refund(10.5);
?>
```
```java
Payment payment = Payment.findById(paymentId);
payment.refund(10.5);
```
```node
mercadopago.payment.refundPartial({ payment_id: id, amount: Number(amount) })
  .then(function (response) {
    //Process response...
  })
  .catch(function (error) {
    //Handle the error ...
  });
```
```ruby
payment = MercadoPago::Payment.find_by_id(paymnentId)
payment.refund(10.5);
```
```curl
curl -X POST \
-H "Content-Type: application/json" \
-H 'Authorization: Bearer ACCESS_TOKEN' \
'https://api.mercadopago.com/v1/payments/:ID/refunds' \
-d '{"amount":10.5}'
```
]]]


### Get the refunds made

You can view the refunds issued for a specific payment with the following request:

[[[
```php
<?php
  $payment = MercadoPago\Payment::find_by_id($payment_id);
  $refunds = $payment->refund();
?>
```
```java
Payment payment = Payment.findById(paymentId);
ArrayList<Refund> refunds = payment.refund();
```
```node
mercadopago.payment.refund(paymentId).then(function(data) {}
  //Do Stuff ..
});
```
```ruby
payment = MercadoPago::Payment.find_by_id(payment_id)
refunds = payment.refund()
```
```curl
curl -X GET \
-H "Content-Type: application/json" \
-H 'Authorization: Bearer ACCESS_TOKEN' \
'https://api.mercadopago.com/v1/payments/:ID'
```
]]]


Response:

```json
{
    "id": PAYMENT_ID,
    ...

    "refunds": [
      {
        "id": 111,
        "payment_id": PAYMENT_ID,
        "amount": 16.98,
        "metadata": {
        },
        "source": {
            "id": "130379930",
            "name": "Firstname Lastname",
            "type": "collector"
        },
        "date_created": "2014-12-04T17:00:03.000-04:00",
        "unique_sequence_number": null
      }
    ]
}
```
