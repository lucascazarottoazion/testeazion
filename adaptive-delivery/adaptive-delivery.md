---
title: "Adaptive Delivery"
---

# Adaptive Delivery

O Azion Adaptive Delivery é um serviço eficaz de detecção de dispositivos, permitindo que você entregue diferentes variações de seu conteúdo para desktop, mobile, tablets e smart TVs, a partir da mesma URL.

Você melhora a experiência de navegação, evitando redirects para variações específicas de seu conteúdo em outras URLs.

A utilização da mesma URL de um conteúdo para todos os dispositivos melhora a qualidade dos compartilhamentos de seus links, garantindo a entrega do conteúdo no formato correto para cada dispositivo do usuário, e melhora o rankeamento de suas páginas nos motores de busca (SEO).

Com Azion Adaptive Delivery você cria seus grupos de dispositivos customizados e configura como deseja que a Azion trate as variações de seu conteúdo de acordo com o dispositivo detectado.

    1. Detecção de dispositivo
    2. Variação de cache do conteúdo por grupo de dispositivos
    3. Regras para comunicação com sua origem
    4. Redirect para mdot

1. ## Detecção de dispositivo

A Azion utiliza o cabeçalho User Agent para detectar o dispositivo do usuário. Você pode customizar a expressão regular para identificação do grupo de dispositivos, de acordo com suas necessidades de negócio.

Por exemplo, para detecção de dispositivos Mobile, você pode utilizar a seguinte expressão regular (ou outra de sua preferência):

`(Mobile|iP(hone|od)|BlackBerry|IEMobile|Kindle|NetFront|Fennec|Minimo|Opera M(obi|ini)|Blazer|Dol(f|ph)in|Skyfire|Zune)`

Você também pode detectar Tablets, Smart TVs e diversos outros grupos de dispositivos por meio de expressões regulares sobre o cabeçalho User Agent.

    Acesse o Real-Time Manager e entre no menu Content Delivery.
    Edite a configuração de Content Delivery para a qual você deseja atribuir um grupo de dispositivos.
    Na aba Device Groups de sua configuração de Content Delivery que possua o Adaptive Delivery ativado, adicione ou edite um grupo de dispositivos atribuindo um nome para o grupo e uma expressão regular sobre o cabeçalho User Agent, como no exemplo acima, e salve a configuração.

Não é necessário customizar um grupo de dispositivos default, geralmente atribuído aos dispositivos Desktop. Você deve criar somente os grupos de dispositivos para os quais possui uma variação de seu conteúdo.
2. ## Variação de cache do conteúdo por grupo de dispositivos

Após criar os grupos de dispositivos de acordo com suas necessidades de negócio, você deve criar ou editar uma política de cache para a qual irá atribuir a variação de cache do conteúdo por grupo de dispositivos.

    Na aba Cache Settings de sua configuração de Content Delivery, crie ou edite uma política de cache.
    Na seção Advanced Cache Key, em Adaptive Delivery, selecione a opção Content varies by some Device Groups (Whitelist).
    Adicione os Device Groups para os quais deseja ativar a variação de cache do conteúdo e salve a configuração.
    Na aba Rules Engine, crie ou edite uma regra na Cache Phase.
    Em Criteria, configure as condições desejadas e, em Behaviors, utilize Set Cache Policy para associar a política de cache que você salvou no passo 3.

Caso seu site ou aplicação web permita que seus usuários mobile possam escolher por visualizar a versão desktop em vez da versão mobile, você deve enviar um Cookie para o browser do usuário e ativar a variação de cache do conteúdo por cookie na Azion (requer Application Acceleration ):

    Na seção Advanced Cache Key da política de cache que você editou no passo 3, em Cache by Cookie selecione a opção Content varies by some Cookies (Whitelist).
    Adicione o nome do cookie que você está utilizando para forçar a visualização da versão desktop.

3. ## Regras para comunicação com sua origem

Sua origem é responsável pela entrega do conteúdo no formato adequado para cada grupo de dispositivos configurado. Você pode utilizar uma única origem para servir todas as variações de seu conteúdo ou, se preferir, múltiplas origens servindo o conteúdo no formato adequado para cada grupo.

Uso de uma origem para servir todos os grupos de dispositivos

Se você utiliza uma origem para servir todos os grupos de dispositivos, você deve configurar a Azion para enviar o nome do dispositivo detectado em um cabeçalho HTTP para sua origem. Para isso:

    Na aba Rules Engine de sua configuração de Content Delivery, crie ou edite uma regra na Request Phase.
    Em Criteria, configure as mesmas condições para as quais você associou a variação de cache de conteúdo anteriormente.
    Em Behaviors, utilize Add Request Header para enviar um cabeçalho HTTP customizado para sua origem, informando o grupo de dispositivos detectado pela Azion, como no exemplo a seguir:

`
X-UA-Device: ${device_group}
`

Uso de múltiplas origens para servir cada grupo de dispositivos

Se você utiliza múltiplas origens para servir diferentes variações de seu conteúdo de acordo com o grupo de dispositivos, você precisará do Application Acceleration para configurar regras condicionais para seleção de cada origem:

    Na aba Rules Engine de sua configuração de Content Delivery com Application Acceleration ativado, crie ou edite uma regra na Request Phase.
    Em Criteria, utilize a variável ${device_group} para realizar a comparação com o nome do grupo do dispositivos detectado.
    Em Behaviors, utilize Set Origin para selecionar a origem que irá servir o grupo de dispositivos que você preencheu no passo 2.
    Repita todos os passos para cada grupo de dispositivos que precisar.

Você pode utilizar a Default Rule para configurar a origem que irá atender seu grupo de dispositivos default.
4. ## Redirect para mdot

Caso seu site ou aplicação utilize um domínio específico para as versões mobile, geralmente associado ao domínio m.yourdomain.com, você pode configurar um redirect condicional ao dispositivo detectado pela Azin, da mesma forma como explicado anteriormente.

    Na aba Rules Engine de sua configuração de Content Delivery com Application Acceleration ativado, crie ou edite uma regra na Request Phase.
    Em Criteria, utilize a variável ${device_group} para realizar a comparação com o nome do grupo de dispositivos detectado.
    Em Behaviors, utilize Redirect To (301 Moved Permanently) para configurar o redirect.
    Preencha o argumento com a URL de destino e salve a configuração.

Você pode utilizar variáveis no argumento de destino de um redirect, por exemplo, configurando seu redirect para trocar o domínio preservando o restante da URI: 
`
http://m.yourdomain.com${uri}
`

Você também pode utilizar essa estratégia, caso deseje preservar o domínio e adicionar o grupo de dispositivos como path em sua URL, por exemplo: 
`
	/${device_group}${uri}. 
`

