# HOMEWORK-SOLUTIONS-6

SupportHandler
Это абстрактный базовый класс для всех обработчиков. Он определяет "скелет" цепочки: у каждого обработчика есть следующий обработчик (next), и метод setNext() позволяет связать текущий обработчик с другим. Метод handle() отвечает за логику прохождения запроса по цепочке: если текущий обработчик не справился (метод process() вернул false), запрос автоматически передаётся дальше. Если дошли до конца и никто не смог обработать, выводится сообщение о необходимости эскалации. Этот класс задаёт общую структуру цепочки ответственности.

FAQBotHandler
Этот класс представляет чат-бота техподдержки, который отвечает за самые простые вопросы. Он переопределяет метод process() и проверяет, равен ли запрос строке "password_reset". Если да — бот сам обрабатывает проблему и выводит:
[FAQBot] Handled password_reset.
Если запрос другой, бот передаёт его дальше по цепочке.

JuniorSupportHandler
Это следующий уровень поддержки — младший специалист. В методе process() он проверяет, равен ли запрос строкам "refund_request" или "billing_issue". Если совпадает — специалист обрабатывает проблему и выводит, например:
[JuniorSupport] Handled refund_request.
Если не совпадает, запрос передаётся следующему обработчику.

SeniorSupportHandle
Это старший инженер или специалист техподдержки, который обрабатывает самые сложные и серьёзные проблемы. В методе process() он ищет запросы "account_ban" или "data_loss". Если нашёл — выводит, например:
[SeniorSupport] Handled account_ban.
Если проблема ему тоже не знакома — выводится сообщение об эскалации:
[SeniorSupport] Cannot handle unknown_bug — escalate manually.

Main
Это стартовый класс программы. Здесь создаются экземпляры обработчиков: faq, junior и senior. С помощью setNext() они выстраиваются в цепочку: бот → младший → старший. В цикле перебирается массив проблем: "password_reset", "refund_request", "account_ban", "unknown_bug".
При запуске и комплияций кода вывод будет таким: 
New Issue: password_reset
[FAQBot] Handled password_reset

New Issue: refund_request
[FAQBotHandler] Passed to next handler.
[JuniorSupport] Handled refund_request

New Issue: account_ban
[FAQBotHandler] Passed to next handler.
[JuniorSupportHandler] Passed to next handler.
[SeniorSupport] Handled account_ban

New Issue: unknown_bug
[FAQBotHandler] Passed to next handler.
[JuniorSupportHandler] Passed to next handler.
[SeniorSupport] Cannot handle unknown_bug — escalate manually.
