В начале поднимаем сервер ipa
vagrant up ipa.otus.lan
далее в ручном режиме производим действия описанные в методичке по установке и конфигурации сервера пароль указываем qwerty123, если указываем другой, то нужно изменить его в провижине
включая момент создания нового пользователя
ipa user-add otus-user --first=Otus --last=User --password
далее стартуем vagrant up
после подключаемся к клиенту, добавляем хосты и проверяем выполнение задания