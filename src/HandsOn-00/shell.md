# Personalitzant la shell

La shell **Zsh** és una altra opció popular, potent, versàtil i altament configurable, amb moltes característiques avançades i funcionalitats addicionals. **Zsh** ofereix una sintaxi similar a la *Bash*, però amb més opcions de personalització i una experiència d'ús millorada.

Per configurar la shell Zsh, segueix els passos següents:

1. Instal·la **Zsh**: Executa la següent comanda com a **usuari (root)** en el terminal per instal·lar **Zsh**:

```sh
~ root@debianlab:~# apt install zsh -y
```

2. Configura **Zsh** com a shell predeterminada del teu usuari: 
```sh
~ jordi@debianlab:~$ chsh -s $(which zsh)
~ Contrasenya:
```

3. Reinicia la sessió o tanca i torna a obrir el terminal per aplicar els canvis.

```sh
jordi@debianlab:~$ exit
ssh jordi@127.0.0.1 -p 2222
This is the Z Shell configuration function for new users,
zsh-newuser-install.
You are seeing this message because you have no zsh startup files
(the files .zshenv, .zprofile, .zshrc, .zlogin in the directory
~).  This function can help you with a few settings that should
make your use of the shell easier.

You can:
(q)  Quit and do nothing.  The function will be run again next time.
(0)  Exit, creating the file ~/.zshrc containing just a comment.
     That will prevent this function being run again.
(1)  Continue to the main menu.
(2)  Populate your ~/.zshrc with the configuration recommended
     by the system administrator and exit (you will need to edit
     the file by hand, if so desired).

--> Seleccionar (2)
debianlab# echo $SHELL
/usr/bin/zsh
```

*Oh My Zsh*, conté una àmplia gamma de funcionalitats i complements que poden ajudar-te a treballar de manera més eficient i productiva en la terminal. 

* **Temes personalitzables**: *Oh My Zsh* ofereix una gran varietat de temes visualment atractius que pots utilitzar per personalitzar l'aparença de la teva terminal. Pots canviar fàcilment el tema per adaptar-lo al teu gust o estil de treball.

* **Complementació d'ordres avançada**: Aquesta funcionalitat suggereix ordres mentre escriviu i us estalvia temps en buscar la sintaxi correcta o recordar noms d'ordres complicats.

* **Gestió de complements**: Hi ha una àmplia col·lecció de complements disponibles que poden millorar la productivitat i la comoditat durant el treball amb la terminal.

* **Configuració personalitzada**: Pots personalitzar la configuració de la terminal segons les teves preferències. Pots canviar el comportament per defecte de la terminal, afegir alias, definir variables d'entorn, crear funcions personalitzades i molt més.

Per instal·lar *Oh My Zsh*, segueix els següents passos:

1. Obre una terminal al teu sistema.
2. Executa la comanda següent per instal·lar *Oh My Zsh*:

```sh
$ sh -c "$(
    curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh
    )"
```

3. Obre el fitxer **~/.zshrc** amb el teu editor de text preferit.

```sh
$ nvim ~/.zshrc
```

4. Modifica les opcions segons les teves preferències. Per trobar informació sobre com configuració pots consultar [ohmyzsh](https://github.com/ohmyzsh/ohmyzsh/) .Pots canviar el tema, afegir complements i configurar variables d'entorn, entre altres coses. 

```sh
# Exemple de configuració personalitzada
ZSH_THEME="agnoster"
plugins=(git)
```

5. Crea un nou fitxer anomenat **shell.md** dins del repositori del handson. Explica la teva configuració personalitzada de la shell. Has d'utilitzar la sintaxi Markdown per a la documentació. Pots consultar la sintaxi Markdown a [Markdown Guide](https://www.markdownguide.org/basic-syntax/).

6. Crea un enllaç simbòlic del fitxer ~/.zshrc al repositori del **handson**. Aquest fitxer et permetra recuperar la teva configuració personalitzada de la shell per altres sistemes.

```sh
~jordi@debianlab:~$ ln -s ~/.zshrc ~/course-X/Setmana0/handson/.zshrc
```

7. Un cop realitzats els canvis, has de fer commit i push al repositori remot amb el següent missatge: *feat: Add shell config*.

```sh
~jordi@debianlab:~/course-X$ git add /Setmana0/handson/.zshrc /Setmana0/handson/shell.md; git commit -m "feat: Add shell config"; git push
```


