Opis projektu:

W ramach tego laboratorium stworzyłam trzy kontenery Docker z serwerami Nginx. Nazwałam je kolejno: web1, web2 oraz web3. Wszystkie kontenery połączyłam w jedną niestandardową sieć typu bridge o nazwie lab11net.

Szczegóły implementacji:

- Utworzyłam katalogi na pliki HTML i logi:
  - htmls/web1, htmls/web2, htmls/web3 – zawierające pliki index.html dla poszczególnych kontenerów,
  - logs/web1, logs/web2, logs/web3 – do przechowywania logów nginx (access.log i error.log).

- W każdym katalogu htmls/webX stworzyłam prostą stronę index.html, np.:
  <h1>Laboratorium 11 - Julia Lasota - web1</h1>

- Utworzyłam sieć Dockera:
  docker network create --driver bridge lab11net

- Uruchomiłam trzy kontenery nginx z podmontowanymi katalogami z plikami html i logami. Kontenery połączyłam z siecią lab11net oraz zmapowałam porty hosta na 8081, 8082 i 8083:

  docker run -d --name web1 --network lab11net -p 8081:80 \
    --mount type=bind,source="ścieżka_do_lab11/htmls/web1",target=/usr/share/nginx/html,readonly \
    --mount type=bind,source="ścieżka_do_lab11/logs/web1",target=/var/log/nginx \
    nginx:latest

  Analogicznie uruchomiłam kontenery web2 (port 8082) i web3 (port 8083).

- Logi nginx są zapisywane w odpowiednich folderach na hoście i można je znaleźć pod ścieżkami:
  logs/web1/access.log
  logs/web1/error.log

  oraz odpowiednio dla web2 i web3.

Sprawdzenie działania:

Po uruchomieniu kontenerów, serwisy są dostępne lokalnie pod następującymi adresami:

http://localhost:8081  (web1)
http://localhost:8082  (web2)
http://localhost:8083  (web3)
