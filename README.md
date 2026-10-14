# ai-engineering-course


## Troubleshooting

### 'ascii' codec can't encode characters

**Что видел:** ошибка при запуске после того, как вписал сломанный ключ.
**Почему:** в GIGACHAT_CREDENTIALS попали русские буквы, а ключ уходит в
HTTP-заголовок, где допустим только латинский текст.
**Что сделал:** вернул настоящий ключ (латиница + цифры). Заодно убедился, что
благодаря fallback скрипт не упал, а ответил через [HuggingFace].

### Invalid credentials format. Please use only base64 credentials (Authorization data, not client secret!)

**Что видел:** ошибка при запуске после того, как вписал случайный набор латинских букв и цифр после первой буквы ключа.
**Почему:** GigaChat ожидают ключ в формате Base64 - строго определённой длины и набора символов. Случайные символы нарушают структуру, и сервер отвергает запрос.
**Что сделал:** исправил ключ. Плюс сработал fallback - скрипт не упал, а ответил через HuggingFace.

### [!] GigaChat недоступен:

**Что видел:** ошибку в виде [!] GigaChat недоступен: (URL('https://ngw.devices.sberbank.ru:9443/api/v2/oauth'), 400, b'{"code":4,"message":"Can\'t decode \'Authorization\' header"}', Headers([('server', 'SynGX'), ('date', 'Fri, 18 Sep 2026 08:52:58 GMT'), ('content-type', 'application/json'), ('content-length', '58'), ('connection', 'keep-alive'), ('vary', 'Origin'), ('vary', 'Access-Control-Request-Method'), ('vary', 'Access-Control-Request-Headers'), ('cache-control', 'no-cache, no-store, max-age=0, must-revalidate'), ('pragma', 'no-cache'), ('expires', '0'), ('x-content-type-options', 'nosniff'), ('strict-transport-security', 'max-age=31536000 ; includeSubDomains'), ('x-frame-options', 'DENY'), ('x-xss-protection', '0'), ('referrer-policy', 'no-referrer'), ('allow', 'GET, POST'), ('strict-transport-security', 'max-age=31536000; includeSubDomains')]))
**Почему:** в .env был указан неверный ключ для доступа к GigaChat
**Что сделал:** вернул корректный Authorization key из личного кабинета GigaChat. Дополнительно убедился, что смена провайдера (fallback на HuggingFace) отработала корректно - при недоступности GigaChat скрипт не упал, а получил ответ от резервного провайдера.