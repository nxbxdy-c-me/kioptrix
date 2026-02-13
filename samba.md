## Машина:  Kioptrix level 1
## Вектор входа:	Уязвимость Samba 2.2.1a (trans2open)
## Инструмент:	Metasploit (exploit/linux/samba/trans2open)
## Результат:  Remote Root Shell. Полная компрометация узла.
## Защита:  Обновление Samba до актуальной версии.

   Сперва сканируем машину через команду nmap -sV -A 10.0.2.5
   
   Флаг -sV покажет версию сервисов, -А выполняет ту же функцию, но вдобавок увидим операционную систему.
   Получаем расширенный ответ, ищем порт 139.
   <img width="721" height="80" alt="smb1" src="https://github.com/user-attachments/assets/db9ee2ec-9e12-45e6-a10b-5f772dc7cdcd" />
   
   На нём присутствует сервис samba, только не указана версия.
   Пропишем команду в msfconsole для определения версии.
   <img width="732" height="211" alt="smb2" src="https://github.com/user-attachments/assets/1c686602-971d-4ce2-8e71-c845054caad9" />

   Определив версию переходим в searchsploit для поиска эксплоита, выбираем первый.
   <img width="561" height="151" alt="smb3" src="https://github.com/user-attachments/assets/835e2792-07d8-4c77-83a5-5634c45c6103" />
   
   Находим эксплоит в msfconsole, прописываем use 1, т.к. linux система.
   <img width="592" height="387" alt="smb4" src="https://github.com/user-attachments/assets/0ebb7618-3f50-4a41-b77e-5b537d317e36" />

   Указываем параметры удалённого хоста, полезную нагрузку и айпи локальной машины, запуск.
   <img width="728" height="163" alt="smb5" src="https://github.com/user-attachments/assets/f7f86412-0438-4861-97e8-9cc5965fe0a5" />

   После выполнения команды run, metasploit успешно отправляет полезную нагрузку и открывает сессию командной строки.
   
   Чтобы подтвердить полную компрометацию системы, выполняем проверку текущего пользователя и его прав:
   1. whoami / id: Проверка текущего пользователя. Видим ответ root (uid=0), что подтверждает получение максимальных               привилегий в системе.
      
      <img width="491" height="219" alt="smb6" src="https://github.com/user-attachments/assets/b5306e3c-1763-4f37-afe8-879e8b664726" />

   3. cat /etc/shadow: В качестве доказательства (Proof of Concept) считываем файл с хэшами паролей, доступ к которому есть        только у суперпользователя.
      <img width="562" height="215" alt="smb7" src="https://github.com/user-attachments/assets/26276b01-b06a-48dc-a320-e35268e6b5d2" />

   Итог: Уязвимость в устаревшей версии Samba позволила удаленно выполнить код с правами root без авторизации. Машина           успешно взломана.
