<!-- TITLE: XML API -->
<!-- SUBTITLE: Данное API позволяет реализовать страницу управления услугами подписки в Личном кабинете провайдера/реселлера, а также добавить услуги Velvica в список уже имеющихся у провайдера услуг. -->

> Обращаем внимание, что как с технической, так и с маркетинговой точки зрения схема интеграции через API является более сложной и ресурсоемкой, чем интеграция через IFRAME. Если есть возможность, лучше воспользоваться схемой с IFRAME. 

# Получение информации об услугах
**Пример запроса**

```text
https://$AG_NAME.ag.$INSTANCE_DOMAIN/2.01/xml?
  ag_uuid=$agUuid&
  ag_timestamp=$agTimestamp&
  ag_sign=$agSign&
  ag_verbose=$agVerbose&
  ag_service_key=$agServiceKey
```
**где:**
* **AGNAME** в имени домена: уникальное имя провайдера/реселлера на платформе Velvica.
* **INSTANCEDOMAIN**: доменное имя сервера Velvica.
* **aguuid**: идентификатор текущего залогиненного абонента в системе. Если этот параметр равен пустой строке или 0, то возвращаются просто все имеющиеся услуги. Если запрос производится из интерфейса оператора техподдержки, то aguuid должен иметь формат: IDabonenta@IDсотрудникаподдержки, например: 121000283738@87654. При этом ID сотрудника поддержки может выбираться оператором по его усмотрению (чаще всего берут первичный ключ из БД сотрудников), но он обязательно должен состоять из одних цифр.
* **agtimestamp**: timestamp формирования текущей страницы (int32).
* **agsign**: значение `md5($AGSECRET + "ЧастьЗапросаМеждуAgUuidВключительноиAgSignИсключая")`. Внимание: при вычислении md5 нужно подставить именно строковое значение части URL запроса, не меняя порядок параметров. В примере выше оно равно `$AGSECRET+"aguuid=$agUuid&agtimestamp=$agTimestamp"`, а для запроса `"/?a=a&b=b&aguuid=123&c=c&d=d&agsign=xxxxx"` значение было бы` $AGSECRET+"aguuid=123&c=c&d=d"`.
* **agverbose**: если 0, то поля verbose не возвращаются (для экономии производительности). Если 1, то возвращается полное содержимое.
* **agservicekey**: если задан, возвращается информация только об услуге с данным ключом (если не задан, то обо всех услугах).

Все параметры должны передаваться URL-кодированными, как принято в протоколе HTTP (т.е. специальные символы представлены в виде %XX).

**Пример ответа**

```xml
<root>
    <code>OK</code>
    <message>Операция выполнена успешно.</message>
    <debug>***Отладочная информация (можно писать в лог-файл; пользователю не показывается).***</debug>
    <response>
        <item key="...">
            <service_key>dr_web_classic</service_key>
            <service_external_id>0123</service_external_id>
            <vendor_id>100422586160685346</vendor_id>
            <vendor_title>Asoft</vendor_title>
            <initial_subscribe_cost>14</initial_subscribe_cost>
            <prolongation_cost>59</prolongation_cost>
            <next_charge_at>2011-06-01T03:00:00+04</next_charge_at>
            <!— Если здесь 0, это архивный сервис: хоть на него и подписан текущий юзер, но новые подписки оформлять на него уже нельзя. -->
            <can_subscribe>1</can_subscribe>
            <link>URL сервиса на витрине</link>
            <title>Антивирус Dr.Web Classic</title>
            <groups>
                <item><group_key>antivirus</group_key><group_title>Антивирусы</group_title></item>
                <item><group_key>dr_web</group_key><group_title>Dr. Web</group_title></item>
                ...
            </groups>
            <description_tiny>
                Краткое описание услуги (1 предложение)
            </description_tiny>

            <verbose_description_short>
                Краткое описание услуги (1 абзац)
            </verbose_description_short>

            <verbose_description_full>
                Подробное описание услуги (несколько абзацев).
                <cut text="Подробная информация">
                    Еще порция информации, скрытая за JavaScript-ссылкой 
                    "подробная информация".
                </cut>
                Еще какой-то текст.
            </verbose_description_full>

            <verbose_oferta>
                Длинный текст оферты использования подписки, которую должен принять пользователь 
                при подключении услуги (пользователю должен быть отображен текст оферты
                и показана галочка "Я принимаю оферту").
            </verbose_oferta>

            <image_large>https://static.ag.rentsoft.ru/path/to/image_width_240</image_large>
            <image_medium>https://static.ag.rentsoft.ru/path/to/image_width_160</image_medium>
            <image_small>https://static.ag.rentsoft.ru/path/to/image_width_60</image_small>

            <screenshots key="screenshots">
                <item key="0">
//static.ag.rentsoft.ru/image/4058af3f944c7ecffa4791a4998ad8fd/130face4108b34a5fee7d7f87f66a401/estMsU1MKUvMS05NiS_uLC5JzU1OLEqNL4hPSUozMjY1Too3NTY0sTBMTbEwSU4xUSu3NTcwUMuwNVBL/SrdNAwMA/
                </item>
                <item key="1">
//static.ag.rentsoft.ru/image/302e422b6ddcb5624cd6c2772a68d4e8/60daa62b0c05816c29cb236c0f8be532/estMsU1MKUvMS05NiS_uLC5JzU1OLEqNL4hPSUozMjY1Too3NTY0sTBMT                
                </item>
             </screenshots>

            <!-- Ссылки для подключения (в общем случае, если продукт доступен в нескольких каналах продаж) -->
            <subscribe_url key="subscribe_url">
                <item key="0">
https://$AG_NAME?provider=rentsoft&person_type=natural&login_source=http%3A%2F%2Farendapo.idport.kz%2F&rs_uri=%2Faction%2Fsubscribe%2Fadvanced_systemcare_pro_kzt%3FSID%3D--SID--%3Ffrom%3Dbuy_button%3Fsettings_val%255Brs_agreed%255D%3D1
               </item>
            </subscribe_url>
             
            <!-- Условия подключения. -->
            <verbose_subscription_conditions key="verbose_subscription_conditions">
<p>Вы подключаете «Advanced SystemCare Pro» <span class="nowrap">за 300 тенге/мес</span>. </p><p class="cost_block">После подключения с Вашего счета будет списано <strong class="nowrap">300 тенге</strong>. Затем Вы получите доступ к установочному файлу, и подписка станет активной.</p>
            </verbose_subscription_conditions>

            <!-- Если услуга еще не подключена, то блок subscription отсутствует. -->
            <subscription>
                <created>2011-05-25T12:32:12+04</created>
                <!— Период подписки: 1 day, 1 mon, 2 mons и пр.. -->
                <period>0</period>
                <!— Если услуга подключена и готова к использованию, то ‘done’, в противном случае - ‘pending’. -->
                <activation_status>0</activation_status>
                <!— Финансово заблокированные услуги рекомендуем выводить наряду с активными, но красным цветом. -->
                <is_financial_locked>0</is_financial_locked>
                <!— Если 1, то кнопка "Удалить" напротив подписки не отображается (например, для случая тарифа "интернет+антивирус"). -->
                <disable_deletion>0</disable_deletion>
 
                <!—
                Следующие поля могут быть пустыми, если услуга находится в процессе синхронизации на сайте
                вендора. Синхронизация может занимать до получаса (в худших случаях), это зависит только
                от вендора и загруженности его серверов. По окончании синхронизации Velvica (или вендор – зависит
                от вендора) высылает пользователю письмо на почту с активационными данными.
                -->
                <link>http://vendors.site.com/path/to/exe32.exe</link>
                <link_64>http://vendors.site.com/path/to/exe64.exe</link_64>
                <activation_details>
                    &lt;b&gt;Логин:&lt;/b&gt; abcdef
                    &lt;b&gt;Пароль:&lt;/b&gt; dh$76w2
                    <!-- 
                    Текст в формате XHTML (используются только тэги: p, b, i, br). Определяет 
                    способ активации услуги (логин, пароль, ключ и т.д.). Заранее формат неизвестен, 
                    т.к. у разных вендоров очень разный формат активационных данных. Некоторые
                    вендоры (например, Panda) вообще не возвращают в Velvica активационные данные,
                    а отправляют их на email самостоятельно. Также бывают и "предактивированные" дистрибутивы,
                    ссылка скачивания которых уже является персональной для пользователя.
                    -->
                </activation_details>

                <usage_details>
                    Текст в формате XHTML. Содержит инструкцию по пользованию услугой (например, 
                    по установке ПО).
                </usage_details>       
            </subscription>

            <history>
                <item>
                    <time>2011-05-25T12:32:12+04</time>
                    <action>activate | financial_lock | deactivate</action>
                    <money_charged>59</money_charged> <!-- Пусто, если action != activate. -->
                    <next_charge_at>2011-06-01T03:00:00+04</next_charge_at> <!-- Пусто, если action != activate. -->
                </item>
                ...
            </history>

***            <debug>
                <item key="...">
                    <subscriptionid>...</subscriptionid>
                    <subscriptionstatus>...</subscriptionstatus>
                    <subscriptionstatusnext>...</subscriptionstatusnext>
                </item>
            </debug>*
        </item>
        ...
    </response>
</root>
```

> Замечание 1: кодировка всех текстов в каждом элементе – стандартный XML-эскейпинг (т.е. "<" превращается в &gt;, ">" – в &lt;, "&" – в &amp; и т.д. – при обратном преобразовании XML-парсер на стороне оператора преобразует эти последовательности обратно в символы, это стандратное для XML поведение). Секция CDATA не применяется. Пример можно видеть, например, в элементе `<activation_details>` ниже.
> Замечание 2: все элементы, имена которых начинается с префикса `"debug"`, а также атрибуты `"key"` элементов `<item>` следует игнорировать. Они предназначены для отладки (довольно удобно будет видеть их в логах) и не участвуют в бизнес-логике. В примере такие элементы обозначены синим цветом.

 
