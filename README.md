# Via Oberta – DGP
Documentació d'integració del servei DGP del Consorci AOC.

# **Índex** #

1. [Introducció]
2. [Transmissions de dades disponibles]
3. [Missatgeria del servei]
   1. [Consulta de dades d’identitat (IDENTITAT_DADES)]
      1. [Petició – dades genèriques]
      2. [Petició – dades específiques]
      3. [Resposta – dades específiques]
   2. Verificació de dades d’identitat (IDENTITAT_VERIFICACIO)
      1. [Petició – dades genèriques]
      2. [Petició – dades específiques]
      3. [Resposta – dades específiques]

# 1 Introducció

Aquest document detalla la missatgeria associada al servei de consulta de dades d&#39;Identitat de la Dirección General de la Policía (en endavant DGP).

Per a poder realitzar la integració cal conèixer prèviament la següent documentació:

- Document de Missatgeria Genèrica de la PCI del Consorci AOC.

# 2 Transmissions de dades disponibles

Les dades disponibles a través del servei són les que es presenten a continuació:

- **EMISSOR**: DGP – Dirección General de la Policía

| **PRODUCTE** | **MODALITAT** | **DESCRIPCIO** |
| --- | --- | --- |
| **DGP_IDENTITAT** | IDENTITAT_DADES  | Consulta de les dades d’identitat sense dades de residència.
| **DGP_IDENTITAT** | IDENTITAT_VERIFICACIO  | Verificació de les dades d’identitat sense dades de residència.

Les modalitats tenen disponible la versió imprimible del resultat de la consulta en format PDF. Per més detalls adreceu-vos a l&#39;apartat _Extensions de missatgeria_ del document de missatgeria genèrica.

# 3 Missatgeria dels serveis

A continuació es detalla la missatgeria corresponent al bloc de dades específiques de les modalitats de consum del producte.

```
L&#39;emissor de les dades requereix que s&#39;informin les dades del funcionari que realitza la consulta. Així, cal informar els següents camps de l&#39;element Funcionario del bloc de dades genèriques:

/Peticion/Funcionario/NombreCompletoFuncionario
/Peticion/Funcionario/NifFuncionario
//SolicitudTransmision/DatosGenericos/Solicitante/Funcionario/NombreCompletoFuncionario
//SolicitudTransmision/DatosGenericos/Solicitante/Funcionario/NifFuncionario
```

## 3.1 Consulta de dades d&#39;identitat (IDENTITAT\_DADES)

Consulta de dades d&#39;identitat.

### 3.1.1 Petició – dades genèriques

| _Element_ | _Descripció_ |
| --- | --- |
//DatosGenericos/Titular/TipoDocumentacion | Tipus de documentació (NIF / DNI, NIE).
//DatosGenericos/Titular/Documentacion | Documentació. <ul><li>NIF / DNI (8 dígits + caràcter de control).</li><li>NIE ([X,Y,Z] + 7 dígits + caràcter de control).</li></ul>
//DatosGenericos/Titular/Apellido1 | Primer cognom del titular. Opcional, si no s&#39;informa cal informar l&#39;any de naixement de les dades especifiques.

### 3.1.2 Petició – dades específiques

| _Element_ | _Descripció_ |
| --- | --- |
/peticioConsultaDadesIdentitat/numeroSuport | Conté informació per realitzar la consulta sobre un determinat número de suport. La codificació del numero de suport, dependrà del tipus de ciutadà:<ul><li>Ciutadà espanyol: correspon al camp IDESP del DNIe: 3 caràcters alfanumèrics + 6 dígits (p.e:AAA123456)</li><li>Ciutadà estranger comunitari: C + 8 dígits (on els dígits es corresponen al número de certificat comunitari)</li><li>Ciutadà estranger: E + 8 dígits (on els dígits coincideixen al número de targeta d&#39;identificació estrangera)</li></ul>
/peticioConsultaDadesIdentitat/anyNaixement | Si no s&#39;especifica l&#39;element Apellido1 de les dades genèriques cal informar aquest camp.

```
El camp numeroSuport serà obligatori si el ciutadà és estranger i no s&#39;ha introduït el NIE en el bloc //DatosGenericos/Titular/Documentacion.

 Cal tenir en compte que el camp que identifica de forma inequívoca a un ciutadà estranger en l&#39;estat espanyol és el TIE. Per tant, si en la consulta s&#39;introdueix el NIE la resposta del sistema pot ser que no sigui l&#39;esperada: es podria donar el cas, per exemple, que es donessin dos registres associats al mateix NIE. Així doncs, per evitar aquestes respostes no esperades, es recomana introduir el TIE en el cas d&#39;un ciutadà estranger.
```

### 3.1.3Resposta – dades específiques

| _Element_ | _Descripció_ |
| --- | --- |
/respostaConsultaDadesIdentitat/peticioConsultaDadesIdentitat | Bloc de dades corresponent a la petició que origina la resposta. |
/respostaConsultaDadesIdentitat/resposta/dadesTitular | Bloc que conté les dades d&#39;una resposta amb dades del titular.
//dadesTitular/numeroSuport | Número suport del titular. La codificació del numero de suport, dependrà del tipus de ciutadà. Més informació a l&#39;apartat de la petició. |
//dadesTitular/nom | Nom del titular de la sol·licitud.
//dadesTitular/primerCognom | Primer cognom del titular de la sol·licitud.
//dadesTitular/segonCognom | Segon cognom del titular de la sol·licitud.
//dadesTitular/nacionalitat | Descripció de la nacionalitat del titular de la sol·licitud.
//dadesTitular/sexe | F: femení / M: masculí / I: indefinit.
//dadesNaixement/data | Data de naixement del titular de la sol·licitud.
//dadesNaixement/localitat | Descripció del municipi de naixement del titular de la sol·licitud.
//dadesNaixement/provincia | Descripció de la província de naixement del titular de la sol·licitud.
//dadesTitular/nomPare | Nom del pare.
//dadesTitular/nomMare | Nom de la mare.
//dadesTitular/dataCaducitat | Data en la que expira el document d&#39;identificació del ciutadà consultat. En el cas de consultar un NIE, la data de caducitat es retornarà buida donat que els NIEs no caduquen (00000000). Pels DNIs permanents es retorna com a data de caducitat el 01/01/9999.
//dniNacionalitzat |
/respostaConsultaDadesIdentitat/resultat/codiResultat | Codi de resultat de l&#39;operació:<ul><li>00: operació realitzada correctament.</li><li>altrament: error realitzant l&#39;operació. Vegeu apartat d&#39;aquest document.</li></ul>
/respostaConsultaDadesIdentitat/resultat/descripcio | Descripció del resultat.

#### 3.1.3.1Codis d&#39;error

A continuació s&#39;especifica la correspondència entre els codi d&#39;error i la descripció associada al mateix per a la operació de verificació d&#39;identitat:

| _Codi_ | _Descripció_ |
| --- | --- |
0A | DNI del titular anul·lat.
0I | Nacionalitzat espanyol. En aquest cas, s&#39;ha d&#39;especificar el DNI.
0Q | Nacionalitzat espanyol però no consta el DNI.
16 | Número de suport erroni. El número de suport no es correspon a la documentació especificada.
59 | El document sol·licitat està en tràmit de renovació, retingut, retirat per autoritat judicial o anul·lat per duplicitat.
67 | DNI no assignat.
68 | Titular no identificat.
81 | Número de suport inexistent.
86 | NIE inexistent.
87 | NIE erroni o duplicat. Existeixen més d&#39;un registre per les dades proporcionades per l&#39;usuari.
0502 | Error realitzant l&#39;operació. El detall de l&#39;error apareix al bloc descripció.

## 3.2Verificació de dades d&#39;identitat (IDENTITAT\_VERIFICACIO)

Verificació de les dades d&#39;identitat. Aquesta consulta substitueix la necessitat de demanar fotocòpia del DNI a les persones físiques en qualsevol tràmit administratiu.

### 3.2.1Petició – dades genèriques

| _Element_ | _Descripció_ |
| --- | --- |
| //DatosGenericos/Titular/TipoDocumentacion | Tipus de documentació (NIF / DNI, NIE).
//DatosGenericos/Titular/Documentacion | Documentació.<ul><li>NIF / DNI (8 dígits + caràcter de control).</li><li>NIE ([X,Y,Z] + 7 dígits + caràcter de control).</li></ul>
//DatosGenericos/Titular/Nombre | Nombre del titular. Opcional, si s&#39;informa es valida.
//DatosGenericos/Titular/Apellido1 | Primer cognom del titular. Opcional, si s&#39;informa es valida.
//DatosGenericos/Titular/Apellido2 | Segon cognom del titular. Opcional, si s&#39;informa es valida.

### 3.2.2Petició – dades específiques

| _Element_ | _Descripció_ |
| --- | --- |
| /peticioVerificacioIdentitat/numeroSuport
 | Número suport del titular. La codificació del número de suport, dependrà del tipus de ciutadà:
- Ciutadà espanyol: correspon al camp IDESP del DNIe: 3 caràcters alfanumèrics + 6 dígits (p.e:AAA123456)
- Ciutadà estranger comunitari:
 C + 8 dígits (on els dígits es corresponen al número de certificat comunitari)
- Ciutadà estranger: E + 8 dígits (on els dígits coincideixen al número de targeta d&#39;identificació estrangera)

 |
| --- | --- |
| /peticioVerificacioIdentitat/sexe | F: femení / M: masculí / I: Indefinit.
 |
| //dadesNaixement/data | Data de naixement del titular de la sol·licitud. Format: AAAAMMDD.

- En el cas d&#39;una cerca per un estranger, només serà obligatori especificar l&#39;any, i s&#39;haurà d&#39;especificar 99 pel dia i mes en cas de ser desconeguts. Per exemple:19749999.
 |
| //dadesNaixement/provincia | Codi de la província de naixement del titular de la sol·licitud. Aquest camp anirà codificat amb els codis de l&#39;INE (2 dígits). Si el ciutadà es estranger el codi de província serà 66:02 Albacete03 Alicante/Alacant04 Almería01 Araba/Álava33 Asturias05 Ávila06 Badajoz07 Balears, Illes08 Barcelona48 Bizkaia09 Burgos10 Cáceres11 Cádiz39 Cantabria12 Castellón/Castelló13 Ciudad Real14 Córdoba15 Coruña, A16 Cuenca20 Gipuzkoa17 Girona18 Granada19 Guadalajara21 Huelva22 Huesca23 Jaén24 León25 Lleida27 Lugo28 Madrid29 Málaga30 Murcia31 Navarra32 Ourense34 Palencia35 Palmas, Las36 Pontevedra26 Rioja, La37 Salamanca38 Santa Cruz de Tenerife40 Segovia41 Sevilla42 Soria43 Tarragona44 Teruel45 Toledo46 Valencia/València47 Valladolid49 Zamora50 Zaragoza51 Ceuta52 Melilla66 Estrangers |
| //dadesNaixement/pais | Codi del país de naixement del titular de la sol·licitud. Aquest bloc va codificat segons la codificació ISO 3166-1 Alpha de la normativa OACI (o ICAO en anglès) que codifica amb 3 caràcters tots els països i nacionalitats.
 |

### 3.2.3Resposta – dades específiques

![](RackMultipart20220102-4-q6pku5_html_ca8b94164b698dd2.png)

| _Element_ | _Descripció_ |
| --- | --- |
| /respostaVerificacioIdentitat/peticioVerificacioIdentitat | Bloc de dades corresponent a la petició que origina la resposta.
 |
| --- | --- |
| /respostaVerificacioIdentitat/resultat/codiResultat
 | Codi de resultat de l&#39;operació:
- 00: operació realitzada correctament.
- altrament: error realitzant l&#39;operació. Vegeu apartat 3.2.3.1 d&#39;aquest document.

 |
| /respostaVerificacioIdentitat/resultat/descripcio
 | Descripció del resultat.
 |

#### 3.2.3.1Codis d&#39;error

A continuació s&#39;especifica la correspondència entre els codi d&#39;error i la descripció associada al mateix per a la operació de verificació d&#39;identitat:

| _Codi_ | _Descripció_ |
| --- | --- |
0A | DNI del titular anul·lat.
0I | Nacionalitzat espanyol. En aquest cas, s&#39;ha d&#39;especificar el DNI.
16 | Número de suport erroni. El número de suport no es correspon a la documentació especificada.
59 | El document sol·licitat està en tràmit de renovació, retingut, retirat per autoritat judicial o anul·lat per duplicitat.
67 | DNI no assignat.
68 | Titular no identificat.
69 | El nom del titular no coincideix amb la dada verificada.
70 | El primer cognom del titular no coincideix amb la dada verificada.
71 | El segon cognom no coincideix amb la dada verificada.
72 | La data de naixement del titular no coincideix amb la dada verificada.
73 | El sexe del titular no coincideix amb la dada verificada.
75 | La província de naixement del titular no coincideix amb la dada verificada.
76 | El país de naixement del titular no coincideix amb la dada verificada.
81 | Número de suport inexistent.
86 | NIE inexistent.
87 | NIE erroni o duplicat. Existeixen més d&#39;un registre per les dades proporcionades per l&#39;usuari.
0502 | Error realitzant l&#39;operació. El detall de l&#39;error apareix al bloc descripció.