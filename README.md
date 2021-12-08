# Cmatrix-clone

Clone em python do programa de terminal cmatrix. Utilizado para introuzir os conceitos de git.

## Índice

## Introdução

### Instalando o git
Para instalar o git em uma distribuição baseada em Debian, utilize esse comando:
```bash
sudo apt install git 
```
Talvez seja necessário atualizar os bancos de dados de aplicações do apt. Para isso, execute esse comando:
```bash
sudo apt update
```
Se tudo der certo, o seguinte comando retornará a versao do git instalada no sistema:
```bash
git --version
```

### Criando a pasta do projeto
Para criar uma nova pasta a partir do terminal, utilize:
```bash
mkdir <nome-da-pasta>
```
Contracao de "make directory", o ```mkdir``` serve para criar uma nova pasta. Para verificar se a pasta foi criada com sucesso, utilize:
``` bash
ls
```
Esse comando listar'a todos os arquivos e pastas que se encontram no diretorio atual. As opcoes ```-a``` e ```-l``` servem para, respectivamente, incluir arquivos ocultos na listagem e exibir em um formato de lista com os modificadores de cada arquivo. Teste:
```bash
ls -al
```
Para navegar at'e o diret'orio que criamos, utilizaremos o comando ```cd```, que significa "change directory":
```bash
cd <nome-da-pasta>
```
'E poss'ivel verificar se est'a na pasta correta utilizando o comando ```pwd```, que significa "print working directory":
```bash
pwd
```
Para criar um novo arquivo vazio, 'e poss'ivel utilizar o comando ```touch```:
```bash
touch <nome-do-arquivo>
```
Caso queira adicionar algo ao arquivo rec'em criado, 'e possivel utilizar o comando ```echo``` e redirecion'a-lo utilizando ```>``` ou ```>>```:
```bash
echo "Isso eh um teste" >> <nome-do-arquivo>
```
Para exibir o conteudo do arquivo de texto, utilize ```cat```:
```bash
cat <nome-do-arquivo>
```
Note que a diferenca entre o ```>``` e o ```>>``` 'e que o primeiro comando apaga todo o conte'udo que est'a dentro do arquivo, enquanto o segundo adiciona ao final do arquivo.

Por fim, para abrir a pasta inteira em um editor de texto (utilizaremos o vscode) utilize:
```bash
code .
```
Note que o ```.``` serve para selecionar a pasta atual e tudo que est'a dentro dela

## Programa Inicial

Antes de mais nada, precisamos inicializar o repositorio git:
```bash
git init
```
Apos executar esse comando, ser'a poss'ivel utilizar o comando ```git status``` para verificar o estado da staging area. Ao rodar esse comando, ser'a poss'ivel ver que o arquivo que criamos tem mudancas que n~ao foram mapeadas pelo git. Por enquanto vamos deixar isso, assim, para que possamos criar a primeira parte do programa, que encontra-se a seguir:
```python
#!/usr/bin/python3
import curses

def main(stdscr):
    stdscr.erase()
    stdscr.addstr(1, 50, "Ola mundo")
    stdscr.getch()

if __name__ == "__main__":
    curses.wrapper(main)
```

Note que a primeira linha do c'odigo serve para que seja poss'ivel executar o script sem explicitamente chamar o interpretador python3, pois ele j'a estar'a identificado no c'odigo. No entanto, se tentarmos rodar o c'odigo agora ele retornar'a um erro indicando que n~ao temos permiss~ao para executar aquele c'odigo. Isso se d'a pois, em Linux, um arquivo criado n~ao possu'i permiss~ao para ser executado imediatamente. 'E necess'ario conferir a ele essa permiss~ao. Isso se d'a utilizando o comando ```chmod```.

```bash
chmod +x <nome-do-arquivo>
```

A opc~ao ```+x``` 'e o que confere o direito de execu'c~ao ("eXecution"). Agora, 'e poss'ivel executar o arquivo utilizando:
```bash
./<nome-do-arquivo>
```

Para registrar essas mudan'cas no reposit'orio, precisamos utilizar o comando ```git add```:
```bash
git add <arquivo1> <arquivo2>
```
Alternativamente, podemos adicionar todos os arquivos da pasta utilizando o ```.```:
```bash
git add .
```

Apos adicionar o arquivo com nosso programa na staging area, essas mudan'cas agora ficam contempladas quando executamos o comando ```git status```. A seguir, vamos tornar essas mudan'cas definitivas utilizando o comando ```git commit```.
```bash
git commit
```
O ```commit``` por padr~ao deve ter uma mensagem explicando o que foi mudado. Para escrever a mensagem, o git abre o editor de texto padr~ao. Para alterar essa configura'c~ao, assim como o nome de usuario e email configurados, utilize os comando a seguir:
```bash
git config --global core.editor 'code --wait'
git config --global user.name "<nome>"
git config --global user.email "<email>"
```

Para visualizar o commit que acaba de ser feito (assim como todos os outros que faremos), utilize o comando ```git log```.

Por fim, vamos modificar nosso c'odigo para que a mensagem exibida na tela seja aleatorizada. O c'odigo atualizado pode ser visto a seguir:
```python
#!/usr/bin/python3
import curses
import string
from random import choices, randrange

MIN_STRING_SIZE = 3
MAX_STRING_SIZE = 25

CHAR_COLLECTION = string.printable

def main(stdscr):
    stdscr.erase()
    line = "".join( choices(CHAR_COLLECTION, 
                    k = randrange(MIN_STRING_SIZE, MAX_STRING_SIZE) ) )
    stdscr.addstr(1, 20, line)
    stdscr.getch()

if __name__ == "__main__":
    curses.wrapper(main)
```

'E poss'ivel unir os comando ```commit``` e ```add``` em um s'o utilizando a op'c~ao ```-a``` do ```commit```:
```bash
git commit -a
```

Conforme a quantidade de commits do reposit'orio vai aumentando, vai ficando mais polu'ida a tela. A opcao ```--oneline``` mitiga esse problema:

```bash
git log --oneline
```

## Trabalhando com branches

Para criar um branch novo em seu repositorio, utilize o seguinte comando:
```bash
git branch <nome-do-branch>
```

A chamada do comando ```git branch``` sem o nome do branch lista os branches dispon'iveis no reposit'orio. Para navegar entre branches (e commits), pode-se utilizar o comando ```git checkout```:
```bash
git checkout <nome-do-branch/codigo-do-commit/tag>
```

Para facilitar a navega'c~ao entre commits, pode-se utilizar tags. Para adicionar um tag a um commit, utilize:
```bash
git tag <nome-do-tag>
```

Para deletar uma branch criada, utiliza-se:
```bash
git branch -d <nome-do-branch>
```
Note que o git n~ao vai permitir que a branch seja deletada caso ela tenha modifica'c~oes que ainda n~ao foram fundidas ao branch principal. Para for'car a remo'c~ao do branch, pode-se utilizar a op'c~ao ```-D``` (CUIDADO).
A seguir, vamos avan'car no nosso c'odigo com a cria'c~ao de uma classe para representar cada linha da cmatrix:
```python
#!/usr/bin/python3
import curses
import string
from random import choices, randrange

MIN_STRING_SIZE = 3
MAX_STRING_SIZE = 25

CHAR_COLLECTION = string.printable

class Line():

    def __init__(self):
        self.line = "".join( choices(CHAR_COLLECTION, 
                            k = randrange(MIN_STRING_SIZE, MAX_STRING_SIZE) ) )

    def print(self, screen):
        screen.addstr(10, 10, self.line)
 

def main(stdscr):
    stdscr.erase()
    l = Line()
    l.print(stdscr)
    stdscr.getch()

if __name__ == "__main__":
    curses.wrapper(main)
```

Fa'ca o commit da mudan'ca e vamos para a pr'oxima etapa, para come'car a exibir a frase verticalmente:

```python
#!/usr/bin/python3
import curses
import string
from random import choices, randrange

MIN_STRING_SIZE = 3
MAX_STRING_SIZE = 25

CHAR_COLLECTION = string.printable

class Line():

    def __init__(self):
        self.line = choices(CHAR_COLLECTION, 
                            k = randrange(MIN_STRING_SIZE, MAX_STRING_SIZE) )

    def print(self, screen):
        for i, c in enumerate(self.line):
            screen.addch(i, 10, c)
 

def main(stdscr):
    stdscr.erase()
    l = Line()
    l.print(stdscr)
    stdscr.getch()

if __name__ == "__main__":
    curses.wrapper(main)
```

Para o pr'oximo passo, vamos adicionar movimento `a tela:

```python
#!/usr/bin/python3
import curses
import string
from random import choices, randrange

MIN_STRING_SIZE = 3
MAX_STRING_SIZE = 25

CHAR_COLLECTION = string.printable

class Line():

    def __init__(self, size):
        self.size = size
        self.next_str = []
        self.line = [' '] * self.size

    def print(self, screen):
        for i, c in enumerate(self.line):
            screen.addch(i, 10, c)

    def update(self):
        if len(self.next_str) > 0:
            self.line.pop()
            self.line = [self.next_str.pop()] + self.line

    def generate_str(self):
        self.next_str = choices( CHAR_COLLECTION, 
                            k = randrange(MIN_STRING_SIZE, MAX_STRING_SIZE) )

 

def main(stdscr):
    h, w = curses.LINES, curses.COLS
    stdscr.timeout(50)
    l = Line(h)
    l.generate_str()
    key = -1
    while key != ord('q'):
        stdscr.erase()
        l.print(stdscr)
        l.update()
        key = stdscr.getch()

if __name__ == "__main__":
    curses.wrapper(main)
````
A seguir, vamos adicionar whitespaces e manter o movimento continuo:
```python
#!/usr/bin/python3
import curses
import string
from random import choices, randrange

MIN_STRING_SIZE = 3
MAX_STRING_SIZE = 25
MAX_CONSEC_WHITESPACES = 15
WHITESPACE_THRESHOLD = 3

CHAR_COLLECTION = string.printable

class Line():

    def __init__(self, size):
        self.size = size
        self.next_str = []
        self.consec_whitespaces = 0
        self.line = [' '] * self.size

    def insert_whitespace(self):
        self.line = [' '] + self.line
        self.consec_whitespaces += 1

    def print(self, screen):
        for i, c in enumerate(self.line):
            screen.addch(i, 10, c)

    def update(self):
        self.line.pop()
        if len(self.next_str) == 0:
            self.insert_whitespace()
            if randrange(self.consec_whitespaces, 
                         MAX_CONSEC_WHITESPACES + WHITESPACE_THRESHOLD) \
                         > MAX_CONSEC_WHITESPACES:
                self.consec_whitespaces = 0
                self.generate_str()
        else:
            self.line = [self.next_str.pop()] + self.line

    def generate_str(self):
        self.next_str = choices( CHAR_COLLECTION, 
                            k = randrange(MIN_STRING_SIZE, MAX_STRING_SIZE) )

 
def main(stdscr):
    h, w = curses.LINES, curses.COLS
    stdscr.timeout(50)
    l = Line(h)
    l.generate_str()
    key = -1
    while key != ord('q'):
        stdscr.erase()
        l.print(stdscr)
        l.update()
        key = stdscr.getch()

if __name__ == "__main__":
    curses.wrapper(main)
```
