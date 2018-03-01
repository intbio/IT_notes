# Instructions for adding personal pages to github-hosted web site (in Russian)

1) логинимся на Github.com
2) делаем Fork репозитория https://github.com/intbio/intbio.github.io
3) в форке удаляем файл CNAME, а в файле _config.yml
меняем значение переменных 
baseurl: "/intbio.github.io"
url: "https://ВАШЛОГИН.github.io"
4) идем в Settings -> в разделе GitHub Pages
меняем None на master branch , нажимаем Save

должна появиться ссылка типа https://ВАШЛОГИН.github.io/intbio.github.io/
по которой будет доступна копия сайта.

на этот сайт вам нужно добавить свою страничку - в раздел Team.
Для этого
1) Загружайте свое фото в директорию assets
2) Редактируйте файл 2team.md добавив абзац про себя по выше имеющимся образцам
3) В папке cv создайте файл своего резюме по имеющемуся там образцу
в шапке файла переменная permalink - определяет по какому относительному адресу будет доступно Ваше резюме.


после того, как сайт с модификациями готов и Ваша копия сайта выглядит нормально,
нажимаете Pull Request -> New pull request
и создаете запрос на изменение основного сайта.
