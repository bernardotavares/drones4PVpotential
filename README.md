**This project README is only avaliable in Portuguese for now.**

## Methodology

Este projeto apresenta uma metodologia simplificada para ser aplicada por uma pessoa ou entidade, com acesso a um drone com as especificações necessárias - nomeadamente no que diz respeito à capacidade de fotografia aérea e, de preferência, com GPS para a referenciação das imagens - para estimar o potencial PV da sua casa, de um edifício de grandes dimensões ou até em escalas maiores, como um bairro, aldeia ou vila, em Portugal:

1. Consultar o guia e mapas para o software [Google Earth](https://www.google.com/earth/download/gep/agree.html) da campanha “[Voa na Boa](http://www.voanaboa.pt/codigo-drone)”, para regras gerais de segurança e determinação das regras do espaço aéreo em que se pretende realizar o levantamento aéreo.

2. Preparação de um plano de voo para [requerimento de autorização para o levantamento aéreo](http://www.aan.pt/subPagina-AAN-001.005.002-formularios) à AAN (sempre necessário).
    - a) Utilização do Google Earth e, idealmente, levantamentos do terreno para definir:
	    - i) Um local de aterragem/descolagem desafogado e com boa linha de vista para o drone a todas as alturas, com atenção também ao alcance máximo do drone.
	    - ii) Em que o trajeto seja desimpedido por obstáculos e de conjuntos de mais de 12 pessoas.
    - b) Utilização adicional de softwares como o [Mission Planner](http://ardupilot.org/planner/docs/common-install-mission-planner.html) ou o [Pix4D](https://pix4d.com/product/pix4dcapture/) Capture para simplificação e otimização do trajeto e da fotografia aérea.

3. Consoante as regras apresentadas, preparar ainda um requerimento de autorização para as seguintes entidades:
    - a) [ANAC](http://voanaboa.pt/formularios) – para autorização de voo aéreo nas zonas indicadas.
    - b) [CNPD](https://www.cnpd.pt/bin/legal/forms.htm) – caso o levantamento aéreo envolva, de alguma forma, a aquisição ou tratamento de dados pessoais em que uma pessoa singular possa ser identificada direta ou indiretamente.

4. Instalar e utilizar o software [Pix4D](https://cloud.pix4d.com/login?next=%2Fdownload%2F) para renderização de um modelo tridimensional:
    - a) Iniciar um novo projeto e selecionar as fotografias georreferenciadas adquiridas pelo drone.
    - b) Correr a primeira (“[Initial Processing](https://support.pix4d.com/hc/en-us/articles/202557439-Step-4-Processing-1-Initial-Processing#gsc.tab=0)”) e a segunda (“[Point Cloud and Mesh](https://support.pix4d.com/hc/en-us/articles/202557349-Step-4-Processing-3-Point-Cloud-and-Mesh#gsc.tab=0)”) etapas de processamento do Pix4D.
        - i) Se o modelo apresentado não for satisfatório, considerar a criação de MTPs.
        - ii) No caso de grandes concentrações de ATPs não desejados que possam interferir com o modelo mesh final e que não se tenham conseguido eliminar calibrando o modelo (usando, por exemplo, os [MTPs](https://support.pix4d.com/hc/en-us/articles/202560189#gsc.tab=0)), eliminar manualmente recorrendo à ferramenta de seleção do Pix4D.
        - iii) Se o processamento for demasiado demorado, considerar adicionar “[áreas de processamento](https://support.pix4d.com/hc/en-us/articles/202558439-Menu-View-rayCloud-Left-sidebar-Layers-Processing-Area#gsc.tab=0)” para os próximos passos. Outros aspetos que afetam o [tempo de processamento](https://support.pix4d.com/hc/en-us/articles/204191535-Processing-Speed#gsc.tab=0) é o hardware, a dimensão do projeto e as opções de output selecionadas.
        - iv) No final, recalcular o modelo.
    - c) Confirmar que um modelo mesh com o formato .obj e uma textura com formato .jpg estão [selecionados](https://support.pix4d.com/hc/en-us/articles/208194103-Menu-Process-Processing-Options-2-Point-Cloud-and-Mesh-3D-Textured-Mesh#gsc.tab=0) no segundo passo de processamento e foram criados na pasta do projeto em [`<project folder>/2_densification/3d_mesh/`](https://support.pix4d.com/hc/en-us/articles/202558549-Output-Files-After-Processing-2-Point-Cloud-and-Mesh#gsc.tab=0).

5. Instalar e utilizar o software [Rhino 5](https://www.rhino3d.com/download/rhino/5/latest) com  [Grasshopper](http://www.grasshopper3d.com/page/download-1) e o plugin [DIVA](http://diva4rhino.com/download), para a análise solar:
    - a) Importar o ficheiro .obj e a textura .jpg.
    - b) Analisar a incidência solar nas superfícies pretendidas:
        - i) Usar o comando “[Explode](http://docs.mcneel.com/rhino/5/help/en-us/commands/explode.htm)” para dividir o modelo em vários mesh.
        - ii) Criar um novo layer e adicionar os mesh das superfícies que se pretendem estudar.
        - iii) Se necessário, criar uma [polyline](http://docs.mcneel.com/rhino/5/help/en-us/commands/polyline.htm) e recorrer ao comando [MeshSplit](http://docs.mcneel.com/rhino/5/help/en-us/commands/meshsplit.htm) para recortar um mesh (usando o polyline como linha de corte).
    - c) Criar um sistema com painéis PV (opcional):
        - i) Criar uma superfície com as dimensões do painel PV.
        - ii) Definir a orientação, inclinação e distância entre fileiras:
            1. Orientação ideal a Sul. Mais a Este para beneficiar a produção de energia de manhã ou para Oeste para beneficiar a tarde.
            2. Inclinação normal em Portugal de 34º. Maior inclinação para beneficiar a produção energética no Inverno, e menor para beneficiar o Verão.
            3. Se o objetivo for a criação de fileiras, estas calculam-se recorrendo à fórmula (para Portugal):
`d=L*(sen(β)/tan⁡(h_0 ) +cos⁡(β) )`
Em que L é a altura do painel e β a sua inclinação. Para Portugal pode-se considerar a altura solar h_0=38,72 graus.
        - iii) Copiar e colar o painel PV original para os locais pretendidos.
            1. Se dispostos em fileiras, criar uma polyline com a dimensão da distância calculada e usa-la na disposição dos painéis. 
    - d) Iniciar o “grasshopper” com o comando com o mesmo nome e preparar o projeto:
        - i) Aceder e fazer download deste repositório. Abrir e o ficheiro com formato .gh no Grasshopper e utilizar como base para o desenvolvimento de novos projetos.
        - ii) Fazer o download dos ficheiros climáticos para [Lisboa](https://energyplus.net/weather-location/europe_wmo_region_6/PRT/PRT_Lisboa.085360_INETI), ou outra região, (através do [website](https://energyplus.net/weather) para o efeito do EnergyPlus) e importa-los para o diretório do DIVA em `…/DIVA/WeatherData`, sendo então possível seleciona-los no campo “Loc” no módulo Rad Map.
    - e) Executar “Run” clicando no botão, para simular cada um dos componentes.

6. Para um cálculo do potencial PV, usar:
`E_PV [MWh] = E_incidente [MWh]*r[%]*PR`
Em que E_incidente é a energia solar incidente total simulada e r[%] a eficiência do painel. O performance ratio (PR) pode ser calculado consoante as características das componentes do sistema ou, para uma estimativa geral, pode-se considerar equivalente a 0,8.

Os softwares utilizados estão disponíveis gratuitamente por um período de experimentação.

## Authors

* **Bernardo Tavares** - *Initial work*

See also the list of [contributors](https://github.com/your/project/contributors) who participated in this project.

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details