# Login laravel với Saml2
### Installation - Composer laravel-saml2
```
composer require aacotroneo/laravel-saml2
```
Với version Laravel (<5.5), cần phải add config dưới đây vào service provider:
**config/app.php**
```
'providers' => [
        ...
    	Aacotroneo\Saml2\Saml2ServiceProvider::class,
]
```
Sau đó chạy lệnh
```
php artisan vendor:publish --provider="Aacotroneo\Saml2\Saml2ServiceProvider"
```
Lệnh này sẽ tạo các files `app/config/saml2_settings.php` & `app/config/saml2/mytestIDP1_IDP_settings.php`, để customize sp và IDP.

### Cấu hình
#### Định nghĩa IDPs
Trong file `config/saml2_settings.php`. Định nghĩa tên các IDP mà hệ thống sử dụng. Ví dụ:

**config/saml2_settings.php**
```
'IDPNames' => ['mytestidp1', 'test', 'myidp2'],
```
#### Cấu hình laravel-saml2 với mỗi IDP được định nghĩa ở trên
Với mỗi IDP cần tạo 1 file cấu hình trong thư mục `app/config/saml2/` theo định dạng `{idp_name}_idp_settings.php`. Ví dụ: `test_idp_settings.php`.
Chỉ nên **copy** từ file mặc định `mytestidp1_idp_settings.php` và sửa. Không nên tự tạo mới.

Trong file idp_settings. Sửa lại biến `$this_idp_env_id`. Biến này đại diện cho mỗi IDP config trong file **.env** . Ví dụ:
**config/saml2/test_idp_settings.php**
```
$this_idp_env_id = 'TEST';
```
#### Cấu hình file .env
Với mỗi biến được tạo trong file IDP config ở trên. Setting tương ứng trong file **.env**. Ví dụ:
```
SAML2_TEST_IDP_ENTITYID=https://app.onelogin.com/saml/metadata/6148882a-976c-48c1-8619-c8c78bcd63a3
SAML2_TEST_IDP_SSO_URL=https://khamluong.onelogin.com/trust/saml2/http-post/sso/6148882a-976c-48c1-8619-c8c78bcd63a3
SAML2_TEST_IDP_SL_URL=https://khamluong.onelogin.com/trust/saml2/http-redirect/slo/1903307
SAML2_TEST_IDP_x509=MIID3zCCAsegAwIBAgIUZrR9V6Q0L+LrqlL0gFUiSku/7mwwDQYJKoZIhvcNAQEFBQAwRjERMA8GA1UECgwIVmlldCBOYW0xFTATBgNVBAsMDE9uZUxvZ2luIElkUDEaMBgGA1UEAwwRT25lTG9naW4gQWNjb3VudCAwHhcNMjIxMTAyMTU1MDU1WhcNMjcxMTAyMTU1MDU1WjBGMREwDwYDVQQKDAhWaWV0IE5hbTEVMBMGA1UECwwMT25lTG9naW4gSWRQMRowGAYDVQQDDBFPbmVMb2dpbiBBY2NvdW50IDCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBANem89pLcx9v5rvY4XmcJcp3MwTmLO46tba97Lwo2eJHblzF0LlcL5lau8SCvCmlDuZFQJcDYuF0w2DWPq3hXaIJgfZR1UCnEec5BiDmIbvehIbutVzlYaN3hYb+TlpBfTqgg+Mo0dVY0H85MXbcTYNZwpR10/5HXWGgfB/7Jz80/gq5NyKPuhmhhM/0vOcSroPGp7TBCBkHKKeZiCsA+fvEH/SUvo/t8wAjcXJ6j3oG4Ww/m+ZMBYJc9tvHmUOq0nEr83tY1JSD2hSrFPTBM4ZlnD4lDCvoF9LM+eXtgFOS670iCylNG3JatxRdwL2A9mhEzlC8J225lze/tjR7HVkCAwEAAaOBxDCBwTAMBgNVHRMBAf8EAjAAMB0GA1UdDgQWBBRPMOFQyv6eSO5pxJnY+TPriDokaDCBgQYDVR0jBHoweIAUTzDhUMr+nkjuacSZ2Pkz64g6JGihSqRIMEYxETAPBgNVBAoMCFZpZXQgTmFtMRUwEwYDVQQLDAxPbmVMb2dpbiBJZFAxGjAYBgNVBAMMEU9uZUxvZ2luIEFjY291bnQgghRmtH1XpDQv4uuqUvSAVSJKS7/ubDAOBgNVHQ8BAf8EBAMCB4AwDQYJKoZIhvcNAQEFBQADggEBAJASmgvAFrsZS17eTmT0SPOc1HrRSQ00c/QYvyG6oGgMoKvBMn9xqq6uzsukA6HqCWreOwgnrnlN5RZKbxFONJ4TiK9PlMfedGGgnbnd7AciZwRt4ua96eiHAYRcq8HB1i8UwxaQ5A/zLufwOiFQtM+sWSpgP2xSSU4ss9Kjaz9aHBPGOXxJqCzHYlRsT3TJWDIQcF/YblqnwSF2vq+0IZvfbdgXa0G/JMW8UGgjWBwGYTXLGbLguh+wRi4i5Yo5LBkiHYN2bJwPtyBOz0js9W9JllaJLr75UPW6wBYPrsgY23Qubb6YgIw+iFu+CNVGiGpG8cQTtOyphbrKEyQ/Efg=
```
Trên là cấu hình đơn giản nhất để login với saml2.
SAML2_TEST_IDP_ENTITYID là id của IDP. ID này là unique trong hệ thống.
SAML2_TEST_IDP_SSO_URL là link login với saml2.
SAML2_TEST_IDP_SL_URL là link logout với saml2.
SAML2_TEST_IDP_x509 là certificate của IDP.

Các thông tin trên được lấy ở hệ thống IDP.

#### Các link cần cấu hình trên IDP
- {routesPrefix}/{IDPName}/logout. Là link logout của SP
- {routesPrefix}/{IDPName}/login. Là link login của SP
- {routesPrefix}/{IDPName}/metadata. Là link metadata của SP. Link này cần cấu hình trên IDP.
- {routesPrefix}/{IDPName}/acs. Là link callback khi login thành công trên IDP. Link này cần cấu hình trên IDP.
- {routesPrefix}/{IDPName}/sls. Là link callback khi logout thành công trên IDP. Link này cần cấu hình trên IDP.

### Sử dụng

#### Login
Khi login thành công. IDP sẽ gọi callback vào `{routesPrefix}/{IDPName}/acs` của SP. SP validate SamlResponse thành công sẽ trigger event. Trong file: **App/Providers/MyEventServiceProvider.php**
```
Event::listen('Aacotroneo\Saml2\Events\Saml2LoginEvent', function (Saml2LoginEvent $event) {
    $messageId = $event->getSaml2Auth()->getLastMessageId();

    $user = $event->getSaml2User();
    $userData = [
        'id' => $user->getUserId(),
        'attributes' => $user->getAttributes(),
        'assertion' => $user->getRawSamlAssertion()
    ];
        $laravelUser = 
        // Logic của từng SP ở đây, có thể tìm user dựa trên user_id. Tạo mới hoặc update user.
        // Sau đó login hệ thống.
        Auth::login($laravelUser);
        // Redirect sau khi login.
});
```

#### Logout
Logout có thể sử lý theo 2 cách:
1. SP chủ động logout, lúc này SP 'nên' thông báo cho IDP để IDP logout.
2. Logout bằng SSO. SP sẽ gọi link logout `{routesPrefix}/{IDPName}/logout`. IDP sẽ trả response về `{routesPrefix}/{IDPName}/sls`.

SP validate SamlResponse thành công sẽ trigger event. Trong file: **App/Providers/MyEventServiceProvider.php**
```
Event::listen('Aacotroneo\Saml2\Events\Saml2LogoutEvent', function ($event) {
    Auth::logout();
    Session::save();
});
```
