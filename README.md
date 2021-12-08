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
