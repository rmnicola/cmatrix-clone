# Cmatrix-clone

Clone em python do programa de terminal cmatrix utilizado para introuzir os conceitos de git.

## Índice

## Introdução

### Instalando o git
Para instalar o git em uma distribuição baseada em Debian, utilize esse comando:
```bash
sudo apt install git 
```
Talvez seja necessário atualizar os bancos de dados de aplicações do apt. Para isso, execute:
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
Contração de "make directory", o ```mkdir``` serve para criar uma nova pasta. Para verificar se a pasta foi criada com sucesso, utilize:
``` bash
ls
```
Esse comando listará todos os arquivos e pastas que se encontram no diretorio atual. As opcoes ```-a``` e ```-l``` servem para, respectivamente, incluir arquivos ocultos na listagem e exibir em um formato de lista com os modificadores de cada arquivo. Teste:
```bash
ls -al
```
Para navegar até o diretório que criamos, utilizaremos o comando ```cd```, que significa "change directory":
```bash
cd <nome-da-pasta>
```
É possível verificar se está na pasta correta utilizando o comando ```pwd```, que significa "print working directory":
```bash
pwd
```
Para criar um novo arquivo vazio, é possível utilizar o comando ```touch```:
```bash
touch <nome-do-arquivo>
```
Caso queira adicionar algo ao arquivo recém criado, é possível utilizar o comando ```echo``` e redirecioná-lo utilizando ```>``` ou ```>>```:
```bash
echo "Isso eh um teste" >> <nome-do-arquivo>
```
Para exibir o conteúdo do arquivo de texto, utilize ```cat```:
```bash
cat <nome-do-arquivo>
```
Note que a diferenca entre o ```>``` e o ```>>``` é que o primeiro comando apaga todo o conteúdo que está dentro do arquivo, enquanto o segundo adiciona ao final do arquivo.

Por fim, para abrir a pasta inteira em um editor de texto (utilizaremos o vscode) utilize:
```bash
code .
```
Note que o ```.``` serve para selecionar a pasta atual e tudo que está dentro dela

## Programa Inicial

Antes de mais nada, precisamos inicializar o repositorio git:
```bash
git init
```
Após executar esse comando, será possível utilizar o comando ```git status``` para verificar o estado da *staging area*. Ao rodar esse comando, poderá ser visto que o arquivo que criamos tem mudanças não mapeadas pelo git. Por enquanto, vamos deixar dessa forma para que possamos criar a primeira parte do programa. Ele encontra-se a seguir:
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

Note que a primeira linha do código serve para que seja possível executar o script sem explicitamente chamar o interpretador python3, pois ele já estará identificado no código. No entanto, se tentarmos rodar o código agora ele retornará um erro indicando que não temos permissão para executar aquele código. Isso se dá pois, em Linux, um arquivo criado não possuí permissão para ser executado imediatamente. É necessário conceder a ele essa permissão. Isso se dá utilizando o comando ```chmod```.

```bash
chmod +x <nome-do-arquivo>
```

A opcão ```+x``` é o que confere o direito de execução (*"eXecution"*). Agora, é possível executar o arquivo utilizando:
```bash
./<nome-do-arquivo>
```

Para registrar essas mudanças no repositório, precisamos utilizar o comando ```git add```:
```bash
git add <arquivo1> <arquivo2>
```
Alternativamente, podemos adicionar todos os arquivos da pasta utilizando o ```.```:
```bash
git add .
```

Após adicionar o arquivo com nosso programa na *staging area*, essas mudanças agora ficam contempladas quando executamos o comando ```git status```. A seguir, vamos tornar essas mudanças definitivas utilizando o comando ```git commit```.
```bash
git commit
```
O ```commit``` por padrão deve ter uma mensagem explicando o que foi mudado. Para escrever a mensagem, o git abre o editor de texto padrão. Para alterar essa configuração, assim como o nome de usuario e email configurados, utilize os comando a seguir:
```bash
git config --global core.editor 'code --wait'
git config --global user.name "<nome>"
git config --global user.email "<email>"
```

Para visualizar o commit que acaba de ser feito (assim como todos os outros que faremos), utilize o comando ```git log```.

Por fim, vamos modificar nosso código para que a mensagem exibida na tela seja aleatorizada. O código atualizado pode ser visto a seguir:
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

É possível unir os comando ```commit``` e ```add``` em um só utilizando a opção ```-a``` do ```commit```:
```bash
git commit -a
```

Conforme a quantidade de commits do repositório vai aumentando, vai ficando mais poluída a tela. A opção ```--oneline``` mitiga esse problema:

```bash
git log --oneline
```

## Trabalhando com branches

Para criar um branch novo em seu repositorio, utilize o seguinte comando:
```bash
git branch <nome-do-branch>
```

A chamada do comando ```git branch``` sem o nome do branch lista os branches disponíveis no repositório. Para navegar entre branches (e commits), pode-se utilizar o comando ```git checkout```:
```bash
git checkout <nome-do-branch/codigo-do-commit/tag>
```

Para facilitar a navegação entre commits, pode-se utilizar tags. Para adicionar um tag a um commit, utilize:
```bash
git tag <nome-do-tag>
```

Para deletar uma branch criada, utiliza-se:
```bash
git branch -d <nome-do-branch>
```
Note que o git não vai permitir que a branch seja deletada caso ela tenha modificações que ainda não foram fundidas ao branch principal. Para forçar a remoção do branch, pode-se utilizar a opção ```-D``` (CUIDADO).
A seguir, vamos avançar no nosso código com a criação de uma classe para representar cada linha da cmatrix:
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

Faça o commit da mudança e vamos para a próxima etapa: começar a exibir a frase verticalmente.

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

Para o próximo passo, vamos adicionar movimento à tela:

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
