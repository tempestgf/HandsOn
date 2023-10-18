# Configurant Git i Github

Git i Github són eines molt utilitzades en el desenvolupament de software. Permeten gestionar els canvis en el codi font i col·laborar amb altres desenvolupadors en un entorn de desenvolupament.Els repositoris i Git tenen tres seccions principals on el teu codi pot existir:

* **Àrea de treball (Working Area)**: Aquesta és l'estructura de treball, on viuen tots els fitxers no rastrejats. Pots afegir contingut nou, modificar o esborrar contingut. Si el contingut que modifiques o esborres és dins del teu repositori, no has de preocupar-te de perdre la teva feina.

* **Àrea d'escenari (Staging Area)**: Aquesta és com una memòria cau. Aquí és on resideixen els fitxers que formaran part del teu següent commit. Pots afegir-hi fitxers o eliminar-ne. L'àrea d'escenari et permet preparar els canvis que vols incloure en el proper commit abans de fer-los permanents.

* **Repositori (Repository)**: Aquesta àrea conté tots els teus commits. Un commit és una instantània de com es veuen la teva àrea de treball i àrea d'escenari en el moment del commit. Es troba dins del directori .git. Els commits són versions registrades del teu projecte i guarden un historial dels canvis fets al llarg del temps.


Un repositori *local* i *remot* en Git es refereixen a dues instàncies del mateix repositori que resideixen en diferents ubicacions.

**Un repositori local** és la còpia del repositori que tens al teu sistema local. Pots  afegir, editar i esborrar fitxers, realitzar commits i realitzar altres operacions sense necessitat d'estar connectat a un repositori remot. 

D'altra banda, **un repositori remot** és una còpia del repositori que resideix en un servidor o un altre sistema remot. Aquest repositori remot serveix com a punt central on múltiples desenvolupadors poden col·laborar i compartir els canvis del projecte. Quan puges els canvis al repositori remot, els altres membres del projecte poden veure, revisar i integrar els teus canvis al seu propi repositori local.

![Font: https://support.nesi.org.nz/hc/en-gb/articles/360001508515-Git-Reference-Sheet](https://support.nesi.org.nz/hc/article_attachments/360004194235/Git_Diagram.svg)

En la imatge es pot observar els dos repositoris *local* i *remote* i les diferent àrees de treball amb les operacions que permet moure els canvis entre els diferents components. 

## Configurant Git

1. Instal·la Git:

```sh
root@debianlab:~# apt install git -y
```

2. Configura el teu nom d'usuari i adreça de correu electrònic en Git:

```sh
jordi@debianlab ~ % git config --global user.name "JordiMateoUdL"
jordi@debianlab ~ % git config --global user.email "jordi.mateo@udl.cat"
```

## Configurant Github

3. Crea un compte a GitHub: Si no tens un compte a GitHub, visita [https://github.com/](https://github.com/) i crea un compte gratuït.


4. Inicia la sessió al teu compte de GitHub en el navegador web.

5. Fes clic a la teva foto de perfil a la cantonada superior dreta i selecciona "Configuració" al menú desplegable.

6. A la pàgina de configuració de GitHub, selecciona "Configuració de desenvolupador" al menú lateral esquerre.

7. A la secció "Accessos personals", fes clic a "Genera un access personal".

* **Note**: os-course
* **Expiration**: No expiration
* **Selected scopes**: 
    * repo:
        * repo:statusAccess commit status
        * repo_deploymentAccess deployment status
        * public_repoAccess public repositories
        * repo:inviteAccess repository invitations
        * security_eventsRead and write security events

8. Desplaça't cap avall i fes clic a "Genera un token".

## Vinculant Git i Github

9. A la terminal de Debian, pots configurar Git per utilitzar el token de GitHub executant la següent comanda:

```sh
git config --global github.token TOKEN
```

Reemplaça *TOKEN* pel token d'accés que has generat al pas anterior. Això emmagatzemarà el token de GitHub en la configuració global de Git al teu sistema Debian.

10. Verifica que el token s'hagi configurat correctament executant:

```sh
git config --global --get github.token
```

11. Visteu aquest enllaç i seguiu les instruccions **[course](https://classroom.github.com/a/LeE0ra_2)**. En primer lloc os indicarà que heu d'acceptar la tasca. Veure imatge:

![](../HandsOn-00/figs/git/link.png)

Un cop cliqueu a acceptar, us apareixerà la següent pantalla:

![](../HandsOn-00/figs/git/accept.png)

12. Espereu uns instants i en el vostre compte de github us apareixerà un nou repositori amb el nom del curs. Aneu al repositori i copieu el link per a clonar-lo. Heu de fer click a **code**. Veure imatge:

![](../HandsOn-00/figs/git/repo.png)


13. Clonarem el repositori plantilla pel desenvolupament del curs:

```sh
git clone x
```
> **OBSERVACIÓ**.
 **x**: és el repositori que us ha generat el professor.
 **password**: és el token 

Recorda modificar *X* pel teu usuari de GitHub.

Git t'hauria de demanar les credencials la primera vegada. Introdueix el teu nom d'usuari de GitHub i, a continuació, el teu token d'accés com a contrasenya. Assegura't de copiar el token complet sense cap modificació. Després de fer-ho, Git clonarà el repositori plantilla al teu sistema Debian i en futures accions no et demanarà les credencials.

**NOTA**: El primer cop que feu un clone la terminal us demanarà les credencials de github. Introduïu el vostre **usuari** i el **token** que heu generat abans.

Us recomano per ampliar contiguts sobre Git i GitHub el curs següent: [https://github.com/mouredev/hello-git](https://github.com/mouredev/hello-git).
