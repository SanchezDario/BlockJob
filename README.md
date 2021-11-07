# `Marketplace BlockJob`

Presentación:
==================

BlockJob es una plataforma pensada en fomentar el trabajo online, el cual representa una gran oportunidad en este tiempo post pandemia, especialmente para países con monedas devaluadas, está basada en la blockchain de Near y sirve como marketplace de servicios profesionales. Por un lado se pueden registrar clientes o empleadores, que deseen contratar y por otro lado profesionales de diversas áreas que deseen ofrecer sus servicios, los cuales se representan mediante un token no fungible, que se compra quedando el pago bloqueado en el contrato y posteriormente, ante la satisfacción del cliente, se deriva el pago al profesional, o en el caso de no estar el cliente satisfecho por el imprevisto que fuere, puede apelar a una intermediación, la cual será realizada por los demás profesionales del área que bloqueen una determinada cantidad de tokens para poder ser parte de los jurados, ambas partes presentarán su evidencia y según la votación se postergará el plazo en caso de no haber concenso o se enviarán los fondos a la parte ganadora. Estando el jurado influenciado a votar adecuadamente por un incentivo económico dado por el hecho de ganar tokens en el caso de votar de acuerdo a la mayoría, o perder parte de su stake en caso de votar dentro de la minoría.
Para ejemplificar supongamos que Alice es una programadora que pasa un KYC para registrarse en BlockJob y Juan es un emprendedor que requiere de un sitio web, por lo cual contrata a Alice, si está conforme con el trabajo no apelará y al terminar el plazo simplemente Alice recibirá los fondos correspondientes, luego Juan podrá dar una valoración en el sitio y ambos irán obteniendo un buen historial dentro de la plataforma. Pero en caso de no estar conforme con el trabajo, ya se que no cumplió sus espectativas o simplemente no fue realizado por Alice, Juan podrá apelar y los fondos quedarán bloqueados hasta realizarse una votación según la condiciones que se explicaron anteriormente, si dicha votación favorece a Juan, el recuperará sus fondos menos una comisión dada como incentivo al jurado, la cual se justifica económicamente por el hecho de tener cierta garantía de la realización del trabajo; caso contrario, será Alice quien reciba el pago, ahora sí de forma íntegra y el incentivo al jurado será otorgado a partir de un fondo propio de la plataforma.
Inicialmente BlockJob tomará una estrategia de nicho, centrándose en la nutrición, área con gran demanda de servicios digitales y donde las mediaciones esperamos sean realmente reducidas, luego se expandirá a la programación, donde efectivamente el mayor contratiempo al contratar online suele darse por disputas ante los trabajos realizado y las condiciones exigidas por los clientes, por lo cual las mediaciones jugarán un papel clave. Posteriormente cualquier servicio con suficiente demanda será incluído en la plataforma.

* Aplicación creada con `yarn create near-app --contract=rust --frontend=react`


Implementación:
================

Para ejecutar este proyecto localmente, debe seguir los siguientes pasos:

Paso 1: Prerrequisitos
------------------------------

1. Asegúrese de haber instalado [Node.js] ≥ 12 (recomendamos usar [nvm])
2. Asegúrese de haber instalado yarn: `npm install -g yarn`
3. Instalar dependencias: `yarn install`
4. Cree una cuenta de prueba NEAR [https://wallet.testnet.near.org/]
5. Instale NEAR CLI globalmente: [near-cli] es una interfaz de línea de comandos (CLI) para interactuar con NEAR blockchain

    yarn install --global near-cli

6. Instale Rust en caso de querer realizar pruebas unitarias sobre el contrato 

    yarn install --global near-cli

Paso 2: Configuración de NEAR CLI
-------------------------------

Configure su near-cli para autorizar su cuenta de prueba creada recientemente:

    near login

Para simplificar el proceso, guarde este id en una variable con el siguiente comando:
ID='cuenta.testnet'
ID2='otra_cuenta.testnet'
Puede verificar con el comando: echo $ID

Paso 3: Implementación del contrato
--------------------------------

Clone el código de BlockJobs e implemente el servidor de desarrollo local con : `yarn build` (consulte` package.json` para obtener una lista completa de `scripts` que puede ejecutar con` yarn`). 
Para compilar y desplegar el contrato generado con `yarn buil` en testnet [https://wallet.testnet.near.org/], ejecutar los siguientes comandos:

    cargo build --target wasm32-unknown-unknown

    near deploy $ID --wasmFile target/wasm32-unknown-unknown/debug/professional_services_marketplace.wasm


📑 Explorando los métodos del contrato
==================

Los siguientes comandos le permiten interactuar con los métodos del contrato inteligente utilizando NEAR CLI.

Comando para inicializar el contrato con la metadata por default: 
--------------------------------------------
    near call $ID new_default_meta '{"owner_id": "'$ID'" }' --accountId $ID

Comando para poner a la venta un servicio:
--------------------------------------------
    near call $ID nft_mint '{"token_id": "0", "receiver_id": "'$ID'", "token_metadata": {}}' --accountId $ID --amount 1

Comando para adquirir un servicio:
--------------------------------------------
    near call $ID buy_nft '{"token_id": "0"}' --accountId $ID2 --gas 300000000000000

Comando para quitar de venta un servicio:
--------------------------------------------
    near call $ID delete_nft '{"token_id": "0"}' --accountId $ID --gas 300000000000000

Comando para consultar todos los servicios a la venta de un usuario:
--------------------------------------------
    near call $ID tokens_of '{"account_id": "'$ID'", "from_index": "0", "limit": 10}' --accountId $ID

Comando para agregar un nuevo profesional:
--------------------------------------------
    near call $ID new_professional '{"account_id": "'$ID'", "category": "dev", "degree": true, "nickname": "user123"}' --accountId $ID

Comando para agregar un nuevo empleador:
--------------------------------------------
    near call $ID new_employer '{"account_id": "'$ID'", "category": "dev"}' --accountId $ID



Pruebas unitarias
--------------------------------
Dentro de la carpeta contract, utilice el siguiente comando para ejecutar las pruebas:

    cargo test -- --nocapture


Interfáz gráfica de la aplicación
--------------------------------
    https://www.figma.com/file/r8cMu2HEm6FDMDVYKo2bc5/MarketPlace?node-id=0%3A1



  [create-near-app]: https://github.com/near/create-near-app
  [NEAR accounts]: https://docs.near.org/docs/concepts/account
  [NEAR Wallet]: https://wallet.testnet.near.org/
  [near-cli]: https://github.com/near/near-cli
