> Docker compose sluk
> `docker compose down`

> Docker compose op
> `docker compose up -d --build`

> Check volumes
> `docker volume ls`

>Open MySQL tui shell
>local-a982a1dfc39274e95571cc9f
>`docker exec -it 3311d40319a7 mysql -u root -p airplate_dk_db_development

>Pull credentials from file
>`docker inspect 3311d40319a7 --format '{{range .Config.Env}}{{println .}}{{end}}'


mysql> select * from Users where username = 'Simon From'\G;
*************************** 1. row ***************************
             user_id: gUfJ8DRkvdb8adwLgn6Yt70yGc62
     organization_id: 551
               email: simon.from@airplate.dk
            username: Simon From
            password: NULL
drone_certificate_id: NULL
           privilege: 1
          subscribed: 1
       session_token: NULL
           map_style: 0
         zone_toggle: 0
      show_encrypted: 0
         totp_secret: NULL
 notification_toggle: 1
                 lng: da
        auth_version: 1
 nature_zones_toggle: 0
  faroe_zones_toggle: 1
1 row in set (0.00 sec)





