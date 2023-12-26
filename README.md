# Cloud-Playing-With-Containers
TryHackMe - Advent of Cyber 3 (2021) | Cloud Playing With Containers



																		TryHackMe - Advent of Cyber 3 (2021) | Cloud Playing With Containers
																		====================================================================



Getting Started
---------------

Docker konteyner alətindən istifadə edərək əmrləri yerinə yetirmək üçün AttackBox-u işə salmalısınız. Konteynerlər Virtual Maşınlara (VM-lərə) bənzər virtuallaşdırma mexanizmidir və konteyner şəkilləri Açıq Konteyner Təşəbbüsü Paylaşma Spesifikasiyası

Docker API - Docker Daemon ilə əlaqə saxlamaq üçün istifadə edilən standart əmrlərlə konfiqurasiya edilmiş Linux maşınında yerli rabitə interfeysi.
Docker Daemon - şəkillər, məlumat həcmləri və digər konteyner artefaktları kimi konteyner komponentləri ilə qarşılıqlı əlaqə yaratmaq üçün maşınınızda (Docker demonu) işləyən bir prosesdir.
Docker Konteyner Şəkil Formatı - nəticədə .tar faylı. Versiya 1 üçün docker təsvir formatı OCI Şəkil Spesifikasiyası ilə ciddi şəkildə uyğun deyildi. Məqsədlərimizə görə, bu, bu məşqdə konteyner şəkilləri ilə qarşılıqlı əlaqəmizi dəyişməyəcək, lakin konteyner şəklinin formatını və məzmununu bir qədər dəyişir.

https://opencontainers.org/
https://github.com/opencontainers/distribution-spec/blob/main/spec.md


Docker Images and Amazon Elastic Container Registry
---------------------------------------------------

Bulud-doğma hesablama mühitində konteynerlər infrastrukturun yerləşdirilməsi üçün ilk seçim həllidir. Virtual maşınlar kimi, konteynerlər buludda işləyən bir çox proqramlar və yerləşdirilən proseslər üçün hesablama materialı kimi xidmət edir.

AttackBox-a daxil olduqdan sonra, AttackBox-da defolt olaraq saxlanılan konteyner şəkillərini görmək üçün aşağıdakı əmri işlədə bilərsiniz:

docker images

aşağıdakı kimi bir çıxışı qaytarmalıdır:

root@ip-10-10-73-235:~# docker images
REPOSITORY                 TAG                 IMAGE ID            CREATED             SIZE
aler9/rtsp-simple-server   latest              1d7214a89fd5        12 months ago       19.8MB
reverse_shell_generator    latest              4e7a5550e754        17 months ago       5.94MB
remnux/ciphey              latest              ec11b47184f6        2 years ago         177MB
rustscan/rustscan          2.0.0               6890f34e17b0        3 years ago         41.6MB
bcsecurity/empire          v3.5.2              cbd0b10f7f55        3 years ago         2.05GB
mpepping/cyberchef         latest              36979d2c2b9e        3 years ago         639MB
fnichol/uhttpd             latest              df0db1779d4d        9 years ago         4.87MB

Docker konteynerləri "repozitoriyalar"da saxlanılır, bunlar Docker demonunun necə çata biləcəyini bildiyi fayl xəritələrinə istinad edir, bunlara konteyner .tar faylları daxildir. Anbardakı hər bir şəkilə şəkil etiketi daxil olacaq və şəkillərə onların etiketindən və ya Şəkil ID-sindən istifadə etməklə istinad edilə bilər.

Misal üçün:

remnux/ciphy:latest

və ya

ec11b47184f6

Grinch Enterprise Attack Infrastructure
---------------------------------------


Biz Grinch Enterprises hücum infrastrukturunu ictimaiyyət üçün açıq olan ehtimal Elastik Konteyner Reyestrinə qədər izlədik:


https://gallery.ecr.aws/h0w1j9u3/grinch-aoc

Siz AttackBox-da aşağıdakı əmri işlətməklə potensial Grinch Enterprises şəklini əldə edə bilərsiniz:


docker pull public.ecr.aws/h0w1j9u3/grinch-aoc:latest

root@ip-10-10-73-235:~# docker pull public.ecr.aws/h0w1j9u3/grinch-aoc:latest
latest: Pulling from h0w1j9u3/grinch-aoc
7b1a6ab2e44d: Pull complete 
7181c3c4941b: Pull complete 
148b30b9ae2d: Pull complete 
6f5a7c388565: Pull complete 
ef099323cb4a: Pull complete 
de5bf7e2abf0: Pull complete 
455d5424d859: Pull complete 
b1ee65a7e02a: Pull complete 
a47021107475: Pull complete 
Digest: sha256:593c79eaaa1a905c533e389b0034022e074969da3936df648172c4efc8d421d8
Status: Downloaded newer image for public.ecr.aws/h0w1j9u3/grinch-aoc:latest
public.ecr.aws/h0w1j9u3/grinch-aoc:latest


Konteyneri işlədə və aşağıdakı əmri işlətməklə onunla əlaqə saxlaya bilərsiniz:

docker run -it public.ecr.aws/h0w1j9u3/grinch-aoc:latest

root@ip-10-10-73-235:~# docker run -it public.ecr.aws/h0w1j9u3/grinch-aoc:latest
$ $ $ $ $ 

$ ilə göstərildiyi kimi konteyner şəklinin içərisində qabıq açacaq. Konteynerə daxil olduqdan sonra kiçik bir kəşfiyyat edə bilərik:


ls -la
$ $ $ $ $ ls -la
total 20
drwxr-xr-x 2 newuser newuser 4096 Oct 21  2021 .
drwxr-xr-x 1 root    root    4096 Oct 21  2021 ..
-rw-r--r-- 1 newuser newuser  220 Feb 25  2020 .bash_logout
-rw-r--r-- 1 newuser newuser 3771 Feb 25  2020 .bashrc
-rw-r--r-- 1 newuser newuser  807 Feb 25  2020 .profile


bu, cari iş qovluğunda müntəzəm faylların və ya alt kataloqların olmadığını göstərir.


Spoiler
-------

Növbəti yoxlamaq üçün yaxşı yer mühit dəyişənləridir - Linux-də və xüsusilə konteynerlər üçün mühit dəyişənləri sirləri və ya digər məlumatları saxlamaq üçün istifadə edilə bilər. iş zamanı konteyneri konfiqurasiya etmək üçün istifadə olunan həssas məlumat.

Buna görə də biz printenv gördüyümüz mühit konfiqurasiyaları haqqında ətraflı öyrənməyə çalışırıq:

$ printenv
HOSTNAME=6b5b7cbe8a3c
HOME=/home/newuser
TERM=xterm
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
api_key=a90eac086fd049ab9a08374f65d1e977
PWD=/home/newuser

və biz təxmin etdiyim bir api_key ilə qarşılaşdıq. Grinch Enterprises geridə qoymaq niyyətində deyil.

api_key=a90eac086fd049ab9a08374f65d1e977


Bonus Challenge
---------------

Konteyner təsviri bir sıra təbəqələrdən ibarətdir - ola bilsin ki, əsas konteyner qatında Grinch Enterprises-in quraşdırma prosesinin bir hissəsi kimi təmizləmədiyi həssas məlumat var. Tərtibatçıların (və təcavüzkarların) proqramlarını (və ya hücum alətlərini) konteynerdə paketləyə bilməsinin əsas səbəblərindən biri, konteynerin tərtibatçıya "dondurmaq" proqram və onun asılılıqlarını qurma prosesinin bir hissəsi kimi təsvirə daxil edin. Quraşdırma prosesi Proqram təminatının İnkişafı Həyat Dövrünün (SDLC) bir hissəsidir, burada tətbiqlər və onların asılılıqları birlikdə paketlənir və paylanma və istifadədən əvvəl sınaqdan keçirilir. a>

Şəkil qurulduqdan sonra konteyner şəklini işə salmaq həmişə qurulma zamanı göstərildiyi kimi eyni konfiqurasiya vəziyyəti ilə nəticələnəcək. Konteyner şəkilləri aDockerfile kimi tanınan mənbə faylından qurulur. Dockerfiles, Docker demonuna konteyner şəklini necə yaratmağı öyrədən yeni sətirdən ayrılmış təlimatların siyahısıdır. Siz Dockerfile arayışında Dockerfaylların necə yazılması ilə bağlı ətraflı izahat oxuya bilərsiniz. Dockerfile nümunəsinə burada> baxa bilərsiniz. Grinch Enterprises vəziyyətində, bizdə orijinal Dockerfile yoxdur - lakin konteyner şəkli ilə bizdə eyni dərəcədə yaxşı bir şey var. Gəlin yeni kataloq yaratmaqla və endirilmiş şəkli .tar faylı kimi yadda saxlamaqla başlayaq.

https://docs.docker.com/engine/reference/builder/

1. Create a new directory: mkdir aoc

2. Change directory to the newly created directory:cd aoc

3. Save the container image as a .tar file: docker save -o aoc.tar public.ecr.aws/h0w1j9u3/grinch-aoc:latest

$ exit
root@ip-10-10-73-235:~# mkdir aoc
root@ip-10-10-73-235:~# cd aoc
root@ip-10-10-73-235:~/aoc# docker save -o aoc.tar public.ecr.aws/h0w1j9u3/grinch-aoc:latest

Şəkli saxladıqdan sonra sıxılmış faylı çıxararaq şəkli daha da yoxlaya bilərik

tar -xf aoc.tar


Qeyd edək ki, əmri yerinə yetirərkən -v (müxtəlif) seçimindən istifadə etdim və siz paketdən çıxarılan müxtəlif faylları görə bilərsiniz:


root@ip-10-10-73-235:~/aoc# tar -xvf aoc.tar
40ad0e404f6065a153d1b4d42e8b315be3504a08c21fadd6e5fde5982b45df18/
40ad0e404f6065a153d1b4d42e8b315be3504a08c21fadd6e5fde5982b45df18/VERSION
40ad0e404f6065a153d1b4d42e8b315be3504a08c21fadd6e5fde5982b45df18/json
40ad0e404f6065a153d1b4d42e8b315be3504a08c21fadd6e5fde5982b45df18/layer.tar
4416e55edf1a706527e19102949972f4a8d89bbe2a45f917565ee9f3b08b7682/
4416e55edf1a706527e19102949972f4a8d89bbe2a45f917565ee9f3b08b7682/VERSION
4416e55edf1a706527e19102949972f4a8d89bbe2a45f917565ee9f3b08b7682/json
4416e55edf1a706527e19102949972f4a8d89bbe2a45f917565ee9f3b08b7682/layer.tar
4cc7bdb0ea56d31f57a373d0e7ce0d633ae86dc327087fccf103c8d97f0cc9c4/
4cc7bdb0ea56d31f57a373d0e7ce0d633ae86dc327087fccf103c8d97f0cc9c4/VERSION
4cc7bdb0ea56d31f57a373d0e7ce0d633ae86dc327087fccf103c8d97f0cc9c4/json
4cc7bdb0ea56d31f57a373d0e7ce0d633ae86dc327087fccf103c8d97f0cc9c4/layer.tar
4f62ae56d8d3b96d5fbe86da8a3f7bf6e9195d360b922cd7b162e17619c50664/
4f62ae56d8d3b96d5fbe86da8a3f7bf6e9195d360b922cd7b162e17619c50664/VERSION
4f62ae56d8d3b96d5fbe86da8a3f7bf6e9195d360b922cd7b162e17619c50664/json
4f62ae56d8d3b96d5fbe86da8a3f7bf6e9195d360b922cd7b162e17619c50664/layer.tar
619ddb982b75f0eb6c9f48624e6a0d20be227e893599d8dea05dbdddc8b14e2b/
619ddb982b75f0eb6c9f48624e6a0d20be227e893599d8dea05dbdddc8b14e2b/VERSION
619ddb982b75f0eb6c9f48624e6a0d20be227e893599d8dea05dbdddc8b14e2b/json
619ddb982b75f0eb6c9f48624e6a0d20be227e893599d8dea05dbdddc8b14e2b/layer.tar
9dedacd92213db743681db2e8d5b3247fd79ce266495d061a381c4c0441ce15d/
9dedacd92213db743681db2e8d5b3247fd79ce266495d061a381c4c0441ce15d/VERSION
9dedacd92213db743681db2e8d5b3247fd79ce266495d061a381c4c0441ce15d/json
9dedacd92213db743681db2e8d5b3247fd79ce266495d061a381c4c0441ce15d/layer.tar
a3c1e603ab4385e0b411423e70314651bb371561c45a2bc90951fa05da9ad3c4/
a3c1e603ab4385e0b411423e70314651bb371561c45a2bc90951fa05da9ad3c4/VERSION
a3c1e603ab4385e0b411423e70314651bb371561c45a2bc90951fa05da9ad3c4/json
a3c1e603ab4385e0b411423e70314651bb371561c45a2bc90951fa05da9ad3c4/layer.tar
aa7f7d1cdeacc3a446e297814a6c13a42006dc8a99baad72c0c50383d69ac551/
aa7f7d1cdeacc3a446e297814a6c13a42006dc8a99baad72c0c50383d69ac551/VERSION
aa7f7d1cdeacc3a446e297814a6c13a42006dc8a99baad72c0c50383d69ac551/json
aa7f7d1cdeacc3a446e297814a6c13a42006dc8a99baad72c0c50383d69ac551/layer.tar
f886f00520700e2ddd74a14856fcc07a360c819b4cea8cee8be83d4de01e9787.json
fa28cd504eaba5e76b168c5149551371fbeb3bc0f51d18485fe401a411c2dd17/
fa28cd504eaba5e76b168c5149551371fbeb3bc0f51d18485fe401a411c2dd17/VERSION
fa28cd504eaba5e76b168c5149551371fbeb3bc0f51d18485fe401a411c2dd17/json
fa28cd504eaba5e76b168c5149551371fbeb3bc0f51d18485fe401a411c2dd17/layer.tar
manifest.json
repositories


Bu fayllar manifest.json faylı istisna olmaqla, müxtəlif konteyner təsviri təbəqələrini təmsil edir. manifest.json "manifest" İçərisində olduğumuz son konteyner şəklini təşkil edən konteyner təsviri təbəqələri. Gəlin "jq" adlı alətdən istifadə edərək bu şəkilə nəzər salaq; "gözəl çap etmək" asan oxunaq üçün çıxış:

Note: On an attack box, jq is now pre-installed and you can skip this step

1. jq quraşdırın:apt install jq -y

root@ip-10-10-73-235:~/aoc# apt install jq -y
2. Manifest.json məzmununu olduqca çap etmək üçün jq istifadə edərək terminala çap edin:cat manifest.json | jq


root@ip-10-10-73-235:~/aoc# cat manifest.json | jq
[
  {
    "Config": "f886f00520700e2ddd74a14856fcc07a360c819b4cea8cee8be83d4de01e9787.json",
    "RepoTags": [
      "public.ecr.aws/h0w1j9u3/grinch-aoc:latest"
    ],
    "Layers": [
      "a3c1e603ab4385e0b411423e70314651bb371561c45a2bc90951fa05da9ad3c4/layer.tar",
      "619ddb982b75f0eb6c9f48624e6a0d20be227e893599d8dea05dbdddc8b14e2b/layer.tar",
      "40ad0e404f6065a153d1b4d42e8b315be3504a08c21fadd6e5fde5982b45df18/layer.tar",
      "aa7f7d1cdeacc3a446e297814a6c13a42006dc8a99baad72c0c50383d69ac551/layer.tar",
      "4f62ae56d8d3b96d5fbe86da8a3f7bf6e9195d360b922cd7b162e17619c50664/layer.tar",
      "9dedacd92213db743681db2e8d5b3247fd79ce266495d061a381c4c0441ce15d/layer.tar",
      "fa28cd504eaba5e76b168c5149551371fbeb3bc0f51d18485fe401a411c2dd17/layer.tar",
      "4416e55edf1a706527e19102949972f4a8d89bbe2a45f917565ee9f3b08b7682/layer.tar",
      "4cc7bdb0ea56d31f57a373d0e7ce0d633ae86dc327087fccf103c8d97f0cc9c4/layer.tar"
    ]
  }
]


Qeyd edək ki, fayldakı ilk məlumat parçası konteyner şəklini yaratmaq üçün istifadə olunan əsas konfiqurasiyaları və əmrləri təmsil edən "Config"

f886f00520700e2ddd74a14856fcc07a36c819b4cea8cee8be83d4de01e9787.json
Bu konfiqurasiya faylı həmçinin qablaşdırılmamış konteyner təsviri kataloqunun kökündə yerləşir:

root@ip-10-10-73-235:~/aoc# ls
40ad0e404f6065a153d1b4d42e8b315be3504a08c21fadd6e5fde5982b45df18  aa7f7d1cdeacc3a446e297814a6c13a42006dc8a99baad72c0c50383d69ac551
4416e55edf1a706527e19102949972f4a8d89bbe2a45f917565ee9f3b08b7682  aoc.tar
4cc7bdb0ea56d31f57a373d0e7ce0d633ae86dc327087fccf103c8d97f0cc9c4  f886f00520700e2ddd74a14856fcc07a360c819b4cea8cee8be83d4de01e9787.json
4f62ae56d8d3b96d5fbe86da8a3f7bf6e9195d360b922cd7b162e17619c50664  fa28cd504eaba5e76b168c5149551371fbeb3bc0f51d18485fe401a411c2dd17
619ddb982b75f0eb6c9f48624e6a0d20be227e893599d8dea05dbdddc8b14e2b  manifest.json
9dedacd92213db743681db2e8d5b3247fd79ce266495d061a381c4c0441ce15d  repositories
a3c1e603ab4385e0b411423e70314651bb371561c45a2bc90951fa05da9ad3c4


və manifest.json ilə eyni şəkildə yoxlana bilər:


root@ip-10-10-73-235:~/aoc# cat f886f00520700e2ddd74a14856fcc07a360c819b4cea8cee8be83d4de01e9787.json | jq
{
  "architecture": "amd64",
  "config": {
    "Hostname": "",
    "Domainname": "",
    "User": "newuser",
    "AttachStdin": false,
    "AttachStdout": false,
    "AttachStderr": false,
    "Tty": false,
    "OpenStdin": false,
    "StdinOnce": false,
    "Env": [
      "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
      "api_key=a90eac086fd049ab9a08374f65d1e977"
    ],
    "Cmd": null,
    "Image": "sha256:035522c2043f6036e879810cfffe0db9665ebb09e1852339231fd805daad5325",
    "Volumes": null,
    "WorkingDir": "/home/newuser",
    "Entrypoint": [
      "sh"
    ],
    "OnBuild": null,
    "Labels": null
  },
  "container": "7b422a5dd0a2a59167ae476fcc18f7ae9a094c02de40b4b4effd42a5d032bae4",
  "container_config": {
    "Hostname": "7b422a5dd0a2",
    "Domainname": "",
    "User": "newuser",
    "AttachStdin": false,
    "AttachStdout": false,
    "AttachStderr": false,
    "Tty": false,
    "OpenStdin": false,
    "StdinOnce": false,
    "Env": [
      "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
      "api_key=a90eac086fd049ab9a08374f65d1e977"
    ],
    "Cmd": [
      "/bin/sh",
      "-c",
      "#(nop) ",
      "ENTRYPOINT [\"sh\"]"
    ],
    "Image": "sha256:035522c2043f6036e879810cfffe0db9665ebb09e1852339231fd805daad5325",
    "Volumes": null,
    "WorkingDir": "/home/newuser",
    "Entrypoint": [
      "sh"
    ],
    "OnBuild": null,
    "Labels": {}
  },
  "created": "2021-10-21T20:31:17.236366166Z",
  "docker_version": "20.10.7",
  "history": [
    {
      "created": "2021-10-16T00:37:47.226745473Z",
      "created_by": "/bin/sh -c #(nop) ADD file:5d68d27cc15a80653c93d3a0b262a28112d47a46326ff5fc2dfbf7fa3b9a0ce8 in / "
    },
    {
      "created": "2021-10-16T00:37:47.578710012Z",
      "created_by": "/bin/sh -c #(nop)  CMD [\"bash\"]",
      "empty_layer": true
    },
    {
      "created": "2021-10-20T16:16:12.499990187Z",
      "created_by": "/bin/sh -c apt-get upgrade && apt-get update"
    },
    {
      "created": "2021-10-20T16:16:46.080121757Z",
      "created_by": "/bin/sh -c apt install curl -y"
    },
    {
      "created": "2021-10-21T20:22:41.837170259Z",
      "created_by": "/bin/sh -c apt install python3 -y"
    },
    {
      "created": "2021-10-21T20:23:42.130217528Z",
      "created_by": "/bin/sh -c apt install pip -y"
    },
    {
      "created": "2021-10-21T20:23:52.8316757Z",
      "created_by": "/bin/sh -c apt install git -y"
    },
    {
      "created": "2021-10-21T20:31:13.639594181Z",
      "created_by": "/bin/sh -c git clone https://github.com/hashicorp/envconsul.git root/envconsul/"
    },
    {
      "created": "2021-10-21T20:31:14.315738313Z",
      "created_by": "/bin/sh -c #(nop) WORKDIR /root/envconsul",
      "empty_layer": true
    },
    {
      "created": "2021-10-21T20:31:14.645450256Z",
      "created_by": "/bin/sh -c #(nop) ADD file:cba528c0d7ba7c0c89ad4ce3e550dc4b3128c2804d4dc75daaf1421759f6d664 in . "
    },
    {
      "created": "2021-10-21T20:31:15.914695012Z",
      "created_by": "/bin/sh -c useradd -ms /bin/bash newuser"
    },
    {
      "created": "2021-10-21T20:31:16.26981869Z",
      "created_by": "/bin/sh -c #(nop)  USER newuser",
      "empty_layer": true
    },
    {
      "created": "2021-10-21T20:31:16.617670519Z",
      "created_by": "/bin/sh -c #(nop) WORKDIR /home/newuser",
      "empty_layer": true
    },
    {
      "created": "2021-10-21T20:31:16.937450858Z",
      "created_by": "/bin/sh -c #(nop)  ENV api_key=a90eac086fd049ab9a08374f65d1e977",
      "empty_layer": true
    },
    {
      "created": "2021-10-21T20:31:17.236366166Z",
      "created_by": "/bin/sh -c #(nop)  ENTRYPOINT [\"sh\"]",
      "empty_layer": true
    }
  ],
  "os": "linux",
  "rootfs": {
    "type": "layers",
    "diff_ids": [
      "sha256:9f54eef412758095c8079ac465d494a2872e02e90bf1fb5f12a1641c0d1bb78b",
      "sha256:500cecccd300e0ed1d2cf9595c1b2d7c4367e21364c2db98ac8952d74c3f1fde",
      "sha256:4808c15f81e439b0593df34aea7c7f811f61bb676cf60cabbf4acf06ab95f41e",
      "sha256:60d27d4a42f954a96e48da0a44b01a75addfe378a9e7775217cf8b2f40216fdf",
      "sha256:9c2b4151672df710fc3b809e4d9ab7f84080edf9104be607302af0368475f08d",
      "sha256:6369024ddd8005bc8621c560ca0ec7a23fee9a5426598c8045de3695ecf07a61",
      "sha256:7b140605c39892e722cd0d18fb1fab4d50329d65a27902dda1b3ec46a742e49f",
      "sha256:33a4a349485916db832c7f6d71f3c2f1139fde6fca626206736fd310eedd392f",
      "sha256:f09c564596bc77e010a4bc4f56ba6f9b19b3bf46a333f9d68a6ef0efa1ee960e"
    ]
  }
}


Manifest konfiqurasiya faylının birinci bölməsi konteyner host sistemində işləmək üçün nəzərdə tutulduğu kimi yekun şəkil konfiqurasiyasından keçir. Bununla belə, bu növbəti bölmə təcavüzkar üçün xüsusi maraq kəsb edir - konteyner şəklinin necə qurulduğu buradadır. Siz hər bir bölmənin əyri mötərizələrlə bölündüyünü görə bilərsiniz və bəzi bölmələrdə "empty_layer": true-i göstərən əlavə xətt var.



"created": "2021-10-21T20:31:17.236366166Z",
"docker_version": "20.10.7",
"history": [
  {
    "created": "2021-10-16T00:37:47.226745473Z",
    "created_by": "/bin/sh -c #(nop) ADD file:5d68d27cc15a80653c93d3a0b262a28112d47a46326ff5fc2dfbf7fa3b9a0ce8 in / "
  },
  {
    "created": "2021-10-16T00:37:47.578710012Z",
    "created_by": "/bin/sh -c #(nop)  CMD [\"bash\"]",
    "empty_layer": true
  },
  {
    "created": "2021-10-20T16:16:12.499990187Z",
    "created_by": "/bin/sh -c apt-get upgrade && apt-get update"
  },
  {
    "created": "2021-10-20T16:16:46.080121757Z",
    "created_by": "/bin/sh -c apt install curl -y"
  },
  {
    "created": "2021-10-21T20:22:41.837170259Z",
    "created_by": "/bin/sh -c apt install python3 -y"
  },
  {
    "created": "2021-10-21T20:23:42.130217528Z",
    "created_by": "/bin/sh -c apt install pip -y"
  },
  {
    "created": "2021-10-21T20:23:52.8316757Z",
    "created_by": "/bin/sh -c apt install git -y"
  },
  {
    "created": "2021-10-21T20:31:13.639594181Z",
    "created_by": "/bin/sh -c git clone https://github.com/hashicorp/envconsul.git root/envconsul/"
  },
  {
    "created": "2021-10-21T20:31:14.315738313Z",
    "created_by": "/bin/sh -c #(nop) WORKDIR /root/envconsul",
    "empty_layer": true
  },
  {
    "created": "2021-10-21T20:31:14.645450256Z",
    "created_by": "/bin/sh -c #(nop) ADD file:cba528c0d7ba7c0c89ad4ce3e550dc4b3128c2804d4dc75daaf1421759f6d664 in . "
  },
  {
    "created": "2021-10-21T20:31:15.914695012Z",


root@ip-10-10-73-235:~/aoc# cd 4416e55edf1a706527e19102949972f4a8d89bbe2a45f917565ee9f3b08b7682/
root@ip-10-10-73-235:~/aoc/4416e55edf1a706527e19102949972f4a8d89bbe2a45f917565ee9f3b08b7682# tar -xvf layer.tar
root/
root/envconsul/
root/envconsul/config.hcl
root@ip-10-10-73-235:~/aoc/4416e55edf1a706527e19102949972f4a8d89bbe2a45f917565ee9f3b08b7682# cat root/envconsul/config.hcl
# This denotes the start of the configuration section for Consul. All values
# contained in this section pertain to Consul.
consul {

  # This is the address of the Consul agent. By default, this is
  # 127.0.0.1:8500, which is the default bind and port for a local Consul
  # agent. It is not recommended that you communicate directly with a Consul
  # server, and instead communicate with the local Consul agent. There are many
  # reasons for this, most importantly the Consul agent is able to multiplex
  # connections to the Consul server and reduce the number of open HTTP
  # connections. Additionally, it provides a "well-known" IP address for which
  # clients can connect.
  address = "127.0.0.1:8500"

  # This controls the retry behavior when an error is returned from Consul.
  # Envconsul is highly fault tolerant, meaning it does not exit in the face
  # of failure. Instead, it uses exponential back-off and retry functions
  # to wait for the cluster to become available, as is customary in distributed
  # systems.
  retry {
    # This enabled retries. Retries are enabled by default, so this is
    # redundant.
    enabled = true

    # This specifies the number of attempts to make before giving up. Each
    # attempt adds the exponential backoff sleep time. Setting this to
    # zero will implement an unlimited number of retries.
    attempts = 12

    # This is the base amount of time to sleep between retry attempts. Each
    # retry sleeps for an exponent of 2 longer than this base. For 5 retries,
    # the sleep times would be: 250ms, 500ms, 1s, 2s, then 4s.
    backoff = "250ms"

    # This is the maximum amount of time to sleep between retry attempts.
    # When max_backoff is set to zero, there is no upper limit to the
    # exponential sleep between retry attempts.
    # If max_backoff is set to 10s and backoff is set to 1s, sleep times
    # would be: 1s, 2s, 4s, 8s, 10s, 10s, ...
    max_backoff = "1m"
  }

  # This block configures the SSL options for connecting to the Consul server.
  ssl {
    # This enables SSL. Specifying any option for SSL will also enable it.
    enabled = true

    # This enables SSL peer verification. The default value is "true", which
    # will check the global CA chain to make sure the given certificates are
    # valid. If you are using a self-signed certificate that you have not added
    # to the CA chain, you may want to disable SSL verification. However, please
    # understand this is a potential security vulnerability.
    verify = false

    # This sets the SNI server name to use for validation.
    server_name = "my-server.com"
  }
}

# This block defines the configuration the the child process to execute and
# manage.
exec {
  # This is the command to execute as a child process. There can be only one
  # command per process.
  command = "/usr/bin/app"

  # This is a random splay to wait before killing the command. The default
  # value is 0 (no wait), but large clusters should consider setting a splay
  # value to prevent all child processes from reloading at the same time when
  # data changes occur. When this value is set to non-zero, Envconsul will wait
  # a random period of time up to the splay value before killing the child
  # process. This can be used to prevent the thundering herd problem on
  # applications that do not gracefully reload.
  splay = "5s"

  env {
    # This specifies if the child process should not inherit the parent
    # process's environment. By default, the child will have full access to the
    # environment variables of the parent. Setting this to true will send only
    # the values specified in `custom_env` to the child process.
    pristine = false

    # This specifies additional custom environment variables in the form shown
    # below to inject into the child's runtime environment. If a custom
    # environment variable shares its name with a system environment variable,
    # the custom environment variable takes precedence. Even if pristine,
    # allowlist, or denylist is specified, all values in this option
    # are given to the child process.
    custom = ["PATH=$PATH:/etc/myapp/bin"]

    # This specifies a list of environment variables to exclusively include in
    # the list of environment variables exposed to the child process. If
    # specified, only those environment variables matching the given patterns
    # are exposed to the child process. These strings are matched using Go's
    # glob function, so wildcards are permitted.
    allowlist = ["CONSUL_*"]

    # This specifies a list of environment variables to exclusively prohibit in
    # the list of environment variables exposed to the child process. If
    # specified, any environment variables matching the given patterns will not
    # be exposed to the child process, even if they are in the allowlist. The
    # values in this option take precedence over the values in the allowlist.
    # These strings are matched using Go's glob function, so wildcards are
    # permitted.
    denylist = ["VAULT_*"]
  }

  # This defines the signal sent to the child process when Envconsul is
  # gracefully shutting down. The application should begin a graceful cleanup.
  # If the application does not terminate before the `kill_timeout`, it will
  # be terminated (effectively "kill -9"). The default value is shown below.
  kill_signal = "SIGTERM"

  # This defines the amount of time to wait for the child process to gracefully
  # terminate when Envconsul exits. After this specified time, the child
  # process will be force-killed (effectively "kill -9"). The default value is
  # "30s".
  kill_timeout = "2s"
}

# This is the signal to listen for to trigger a graceful stop. The default
# value is shown below. Setting this value to the empty string will cause it
# to not listen for any graceful stop signals.
kill_signal = "SIGINT"

# This is the log level. If you find a bug in Envconsul, please enable debug or
# trace logs so we can help identify the issue. This is also available as a
# command line flag.
log_level = "warn"

# This is the maximum interval to allow "stale" data. By default, only the
# Consul leader will respond to queries; any requests to a follower will
# forward to the leader. In large clusters with many requests, this is not as
# scalable, so this option allows any follower to respond to a query, so long
# as the last-replicated data is within these bounds. Higher values result in
# less cluster load, but are more likely to have outdated data.
max_stale = "10m"

# This is the path to store a PID file which will contain the process ID of the
# Envconsul process. This is useful if you plan to send custom signals
# to the process.
pid_file = "/path/to/pid"

# This specifies a prefix in Consul to watch. This may be specified multiple
# times to watch multiple prefixes, and the bottom-most prefix takes
# precedence, should any values overlap. Prefix blocks without the path
# defined are meaningless and are discarded. If prefix names conflict with
# secret names, secret names will take precedence.
prefix {
  # This tells Envconsul to use a custom formatter when printing the key. The
  # value between `{{ key }}` will be replaced with the key.
  format = "custom_{{ key }}"

  # This tells Envconsul to use a custom formatter when printing the key. The
  # value after "replaceKey" in  `{{ key | replaceKey `actualKey` `expectedKey` }}` will be replaced with the next value.
  # You could replace more then one key.
  format = "custom_{{ key | replaceKey `actualKey1` `expectedKey1` | replaceKey `actualKey2` `expectedKey2` }}"  

  # This tells Envconsul to not prefix the keys with their parent "folder".
  # The default for `prefix` (consul) is true, the default for `secret` (vault)
  # is false. The differing defaults is to maintain backward compatibility.
  no_prefix = false

  # This is the path of the key in Consul or Vault from which to read data.
  # The path field is required or the config block will be ignored.
  path = "foo/bar"

  # This tells Envconsul to use a custom formatter when building the path for
  # the key from which to read data, in this case reading an environment
  # variable and putting it into the path.
  path = "foo/{{ env \"BAR\" }}"
}

# This tells Envconsul to not include the parent processes' environment when
# launching the child process.
pristine = false

# This is the signal to listen for to trigger a reload event. The default
# value is shown below. Setting this value to the empty string will cause it
# to not listen for any reload signals.
reload_signal = "SIGHUP"

# This tell Envconsul to remove any non-standard values from environment
# variable keys and replace them with underscores.
sanitize = false

# This specifies a secret in Vault to watch. This may be specified multiple
# times to watch multiple secrets, and the bottom-most secret takes
# precedence, should any values overlap. Secret blocks without the path
# defined are meaningless and are discarded. If secret names conflict with
# prefix names, secret names will take precedence.
secret {
  # See `prefix` as they are the same options.
}

# This block defines the configuration for connecting to a syslog server for
# logging.
syslog {
  # This enables syslog logging. Specifying any other option also enables
  # syslog logging.
  enabled = true

  # This is the name of the syslog facility to log to.
  facility = "LOCAL5"
}

# This tells Envconsul to convert environment variable keys to uppercase (which
# is more common and a bit more standard).
upcase = false

# This denotes the start of the configuration section for Vault. All values
# contained in this section pertain to Vault.

vault {
  # This is the address of the Vault leader. The protocol (http(s)) portion
  # of the address is required.
  address = "https://vault.service.consul:8200"

  # This is a Vault Enterprise namespace to use for reading/writing secrets.
  #
  # This value can also be specified via the environment variable VAULT_NAMESPACE.
  namespace = "foo"

  # This is the token to use when communicating with the Vault server.
  # Like other tools that integrate with Vault, Envconsul makes the
  # assumption that you provide it with a Vault token; it does not have the
  # incorporated logic to generate tokens via Vault's auth methods.
  #
  # This value can also be specified via the environment variable VAULT_TOKEN.
  token = "7095b3e9300542edadbc2dd558ac11fa"

  # This tells Envconsul to load the Vault token from the contents of a file.
  # If this field is specified:
  # - by default Envconsul will not try to renew the Vault token, if you want it
  # to renew you will need to specify renew_token = true as below.
  # - Envconsul will periodically stat the file and update the token if it has
  # changed.
  # vault_agent_token_file = "/path/to/vault/agent/token/file"


  # This tells Envconsul that the provided token is actually a wrapped
  # token that should be unwrapped using Vault's cubbyhole response wrapping
  # before being used. Please see Vault's cubbyhole response wrapping
  # documentation for more information.
  unwrap_token = true

  # This option tells Envconsul to automatically renew the Vault token given.
  # If you are unfamiliar with Vault's architecture, Vault requires tokens be
  # renewed at some regular interval or they will be revoked. Envconsul will
  # automatically renew the token at half the lease duration of the token. The
  # default value is true (exception below), but this option can be disabled if
  # you want to renew the Vault token using an out-of-band process.
  # 
  # There is an exception to the default such that if vault_agent_token_file is
  # set, either from the command line or the above option, renew_token defaults
  # to false to avoid renewal conflicts between envconsul and vault-agent.
  #
  # Note that secrets specified as a prefix are always renewed, even if this
  # option is set to false. This option only applies to the top-level Vault
  # token itself.
  renew_token = true

  # This section details the retry options for connecting to Vault. Please see
  # the retry options in the Consul section for more information (they are the
  # same).
  retry {
    # ...
  }

  # This section details the SSL options for connecting to the Vault server.
  # Please see the SSL options in the Consul section for more information (they
  # are the same).
  ssl {
    # ...
  }

# This specifies a service in Consul to watch. This may be specified multiple
# times to watch multiple prefixes, and the bottom-most service takes
# precedence, should any values overlap.
service {
  # This is the query of the service in Consul from which to read data.
  query = "my-service"

  # This tells Envconsul to use a custom formatter when printing the key. The
  # value between `{{ key }}` and `{{ service }}` will be replaced with the key
  # and service name. Default format `{{ service }}/{{ key }}`
  format_id = "pg/{{ key }}"
  format_name = "pg/{{ key }}"
  format_address = "pg/host"
  format_tag = "pg/{{ key }}"
  format_port = "pg/{{ key }}"
}

# This is the quiescence timers; it defines the minimum and maximum amount of
# time to wait for the cluster to reach a consistent state before relaunching
# the app. This is useful to enable in systems that have a lot of flapping,
# because it will reduce the the number of times the app is restarted.
wait {
  min = "5s"
  max = "10s"
}

root@ip-10-10-73-235:~/aoc/4416e55edf1a706527e19102949972f4a8d89bbe2a45f917565ee9f3b08b7682# cat root/envconsul/config.hcl | grep 'token'
  # This is the token to use when communicating with the Vault server.
  # assumption that you provide it with a Vault token; it does not have the
  # incorporated logic to generate tokens via Vault's auth methods.
  token = "7095b3e9300542edadbc2dd558ac11fa"
  # This tells Envconsul to load the Vault token from the contents of a file.
  # - by default Envconsul will not try to renew the Vault token, if you want it
  # to renew you will need to specify renew_token = true as below.
  # - Envconsul will periodically stat the file and update the token if it has
  # vault_agent_token_file = "/path/to/vault/agent/token/file"
  # This tells Envconsul that the provided token is actually a wrapped
  # token that should be unwrapped using Vault's cubbyhole response wrapping
  unwrap_token = true
  # This option tells Envconsul to automatically renew the Vault token given.
  # If you are unfamiliar with Vault's architecture, Vault requires tokens be
  # automatically renew the token at half the lease duration of the token. The
  # you want to renew the Vault token using an out-of-band process.
  # There is an exception to the default such that if vault_agent_token_file is
  # set, either from the command line or the above option, renew_token defaults
  # token itself.
  renew_token = true


What command will list container images stored in your local container registry?
Answer: docker images

What command will allow you to save a docker image as a tar archive?
Answer: docker save


What is the name of the file (including file extension) for the configuration, repository tags, and layer hash values stored in a container image?
Answer: manifest.json


What is the token value you found for the bonus challenge?
Answer: 7095b3e9300542edadbc2dd558ac11fa



