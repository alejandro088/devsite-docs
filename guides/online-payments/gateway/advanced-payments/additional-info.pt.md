---
sites_supported:
  - mla
  - mco
indexable: false
---

# Informações adicionais

> WARNING
>
> Contato comercial necessário
>
> Você só pode integrar este produto se o seu contato comercial compartilhar todas as informações necessárias para isso.

É possível enviar informações que podem melhorar a análise de prevenção de fraude. Para isso, deverá enviar o campo `additional_info` com informações do pagador e o endereço de envio. Quanto mais informações forem enviadas, melhor será a taxa de conversão dos pagamentos.

Além disso, é possível enviar o objeto `metadata` com informações relevantes ao negócio, que o vendedor queira adicionar. As informações contidas neste objeto são do tipo chave/valor.

#### Request
```curl
curl -X POST \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer APPLICATION_TOKEN' \
    'https://api.mercadopago.com/v1/advanced_payments' \
```

#### Body
```json
{
  "payments": [
    ...
  ],
  "payer": {
    "email": "test_user_p@testuser.com"
  },
  ...
  "additional_info": {
    "shipments": {
      "receiver_address": {
        "zip_code": "1111",
        "street_name": "AvenidaUnoytrescuartos",
        "street_number": 123,
        "floor": 4,
        "apartment": "C"
      }
    },
    "payer": {
      "address": {
        "street_name": "CalleCuatro",
        "street_number": "342"
      }
    }
  },
  "metadata": {
    "key1": "value1",
    "key2": "value2"
  }
}
```
