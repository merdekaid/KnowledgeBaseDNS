---
title: Genel Bakış
sidebar_position: 1
---

## AdGuard DNS API

AdGuard DNS, uygulamalarınızı entegre etmek için kullanabileceğiniz bir REST API sağlar.

## Kimlik Doğrulama

### Generate Access token

Make a POST request for the following URL with the given params to generate the `access_token`:

`https://api.adguard-dns.io/oapi/v1/oauth_token`

| Parametre         | Açıklama                                                                       |
|:----------------- |:------------------------------------------------------------------------------ |
| **kullanıcı adı** | Hesap e-postası                                                                |
| **parola**        | Hesap parolası                                                                 |
| mfa_token         | İki Faktörlü kimlik doğrulama belirteci (hesap ayarlarında etkinleştirilmişse) |

Yanıt olarak hem `access_token` hem de `refresh_token` alırsınız.

- `access_token` süresi belirli bir saniye sonra dolar (yanıttaki `expires_in` parametresiyle temsil edilir). `refresh_token` kullanarak yeni bir `access_token` oluşturabilirsiniz (Bakınız: `Yenileme Belirtecinden Erişim Belirteci Oluşturma`).

- `refresh_token` kalıcıdır. To revoke a `refresh_token`, refer: `Revoking a Refresh Token`.

#### Örnek istek

```bash
$ curl 'https://api.adguard-dns.io/oapi/v1/oauth_token' -i -X POST \
    -H 'Content-Type: application/x-www-form-urlencoded' \
    -d 'username=user%40adguard.com' \
    -d 'password=********' \
    -d 'mfa_token=727810'
```

#### Örnek yanıt

```json
{
  "access_token": "jTFho_aymtN20pZR5RRSQAzd81I",
  "token_type": "bearer",
  "refresh_token": "H3SW6YFJ-tOPe0FQCM1Jd6VnMiA",
  "expires_in": 2620978
}
```

### Yenileme Belirtecinden Erişim Belirteci Oluşturma

Erişim belirteçlerinin geçerliliği sınırlıdır. Once it expires, your app will have to use the `refresh token` to request for a new `access token`.

Make the following POST request with the given params to get a new access token:

`https://api.adguard-dns.io/oapi/v1/oauth_token`

| Parametre         | Açıklama                                                                 |
|:----------------- |:------------------------------------------------------------------------ |
| **refresh_token** | `REFRESH TOKEN` kullanılarak yeni bir erişim belirteci oluşturulmalıdır. |

#### Örnek istek

```bash
$ curl 'https://api.adguard-dns.io/oapi/v1/oauth_token' -i -X POST \
    -H 'Content-Type: application/x-www-form-urlencoded' \
    -d 'refresh_token=H3SW6YFJ-tOPe0FQCM1Jd6VnMiA'
```

#### Örnek yanıt

```json
{
  "access_token": "xQnT7GYT6Ag--3oY_EcOOdXe-I0",
  "token_type": "bearer",
  "refresh_token": "H3SW6YFJ-tOPe0FQCM1Jd6VnMiA",
  "expires_in": 2627999
}
```

### Revoking a Refresh Token

To revoke a refresh token, make the following POST request with the given params:

`https://api.adguard-dns.io/oapi/v1/revoke_token`

#### İstek Örneği

```bash
$ curl 'https://api.adguard-dns.com/oapi/v1/revoke_token' -i -X POST \
    -d 'token=H3SW6YFJ-tOPe0FQCM1Jd6VnMiA'
```

| Parametre         | Açıklama                               |
|:----------------- |:-------------------------------------- |
| **refresh_token** | `REFRESH TOKEN` which is to be revoked |

### API'ye erişim

Erişim ve yenileme belirteçleri oluşturulduktan sonra, başlıktaki erişim belirtecini geçirilerek API çağrıları yapılabilir.

- Başlık adı `Authorization` olmalıdır
- Başlık değeri `Bearer {access_token}` olmalıdır

## API

### Referans

Lütfen referans yöntemlerine [buradan](private-dns/api/reference.md) bakın.

### OpenAPI özellikleri

OpenAPI specification is available at [https://api.adguard-dns.io/static/swagger/openapi.json][openapi].

Kullanılabilir API yöntemlerinin listesini görüntülemek için farklı araçlar kullanabilirsiniz. Örneğin bu dosyayı [https://editor.swagger.io/][swagger] adresinde açabilirsiniz.

## Geri Bildirim

Bu API'nin yeni yöntemlerle genişletilmesini istiyorsanız, lütfen `devteam@adguard.com` adresine e-posta gönderin ve nelerin eklenmesini istediğinizi bize bildirin.

[openapi]: https://api.adguard-dns.io/static/swagger/openapi.json
[swagger]: https://editor.swagger.io/
