# Terrain Population Centres
Terrain Population Centres represent inhabited settlements and significant concentrations of civilian population within the World. Population Centres are geographic Areas created using the Area Mapping Primitive in the MapMaker.

Population Centres provide the geographic foundation for the simulation of civilian population, settlement, demographics, culture, economics, political organisation and military significance.

A Terrain Population Centre represents the settlement itself rather than simply the population residing within it. Terrain Population Centres include:
- Village
- Town
- City
- Capital City
  
Population Centres may also contain or overlap with Terrain Objects representing roads, railways, rivers, forests, agricultural land and other geographic features.

## Terrain Population Centres Attributes
Every Population Centre possesses a defined set of physical, demographic, economic, social and administrative attributes. These attributes determine the characteristics of the Population Centre and provide data to the relevant simulation systems. Population Centre attributes are:

### Terrain Population Centres Base Attributes
Population Centres utilise the same attributes also used by [Terrain Objects](TerrainObjects.md). These attributes allow Population Centres to participate directly in relevant military and geographic simulation systems. Base Attributes include:

- Height
- Movement Effect
- Visibility Effect
- Hit Probability Effect
- Health Points
  
These attributes function according to the definitions established within [Terrain Objects](TerrainObjects.md). However, other Population Centre Attributes, such as Population Density, has a modification impact/uplift on Base Attributes. 

### Demographic Attributes
Demographic Attributes describe the composition, distribution and development of the civilian population within a Population Centre. Population Centres maintain their own demographic conditions, which in turn make up the demographic characteristics of their Nation. This allows different settlements within the same Nation to develop substantially different demographic profiles, building different conditions for the Event & Scenario System as well as the Nation's Overview information. 

Demographic Attributes include:
- Population: Population represents the number of civilian inhabitants residing within the Population Centre.
- Age Distribution: 
- Birth Rate:
- Death Rate:
- Immigration
- Emigration:
- Refugees:
- Laborforce:
- Dependents 
- Average Education:
- Employment Rate
- Needs


- Population Density;
- Population Types;


Employment;
Household Composition;
Wealth Distribution;
Standard of Living;
and Population Growth.


#### Population
Population represents the number of civilian inhabitants residing within the Population Centre. Population is dynamic and changes over time as a result of other Attributes. These Attributes are:

- Age Distribution
- Birth Rate
- Death Rate
- Immigration
- Emigration
- Refugees
- Standard of Living

Population is therefore not a static map value. A Population Centre may grow, decline, become depopulated or experience substantial demographic transformation throughout the simulation. For instance, a battle within a Terrain Population Centre or even close to it may dynamically increase Death Rate, lower Birth Rate, increase Emigration and lower Standard of Living. An airstrike, artillery or INSERT on Units entrenched between a population centre will also kill civilians depending on the ammunition, missile or projectile's Circular error probable (CEP). Likewise, it may also create Refugees, who flee to the closest Terrain Population Centre in a neighbouring nation. 

#### Age Distribution
Age Distribution refers to the ages of the Population Attribute. Age Distribution plays a key role in determining Laborforce and Dependents, both of which are determined by the Player's nation's legislation and FINISH. It also plays a role in Death Rate, wherein the '64 and Over' category FINISH. Age Distribution is divided into the following categories:

- 0–14 years: Representing Children, who are Default Dependents. Legislation options, per the Legislation System within the larger Government System, can enforce Child Labour, wherein Children become apart of the Laborforce.
- 15–64 years: Representing Working Age Adults who may be Employed, wherein they make up the Laborforce, or Unemployed, wherein they are Dependents. 
- 65 and over: Representing Retirees, who are Default Dependents. Legislation options, per the Legislation System within the larger Government System, can enforce Late Retirement and other Labor Laws that adds to the Laborforce.

Age Distribution is influenced by the following Attributes:
- Population
- Birth Rate 


#### Birth Rate 
FINISH


#### Birth Rate 
FINISH



#### Average Education
Education represents the average education level of the Labor Force in a Population Centre. Education Levels are as follows:
- No Formal Education
- Primary Education
- Secondary Education
- Higher Education
- Advanced Research Education

Education is advanced progressively within a Population Centre by considering the following:

- Standard of Living
- Population Happiness
- Relevant 'Civil Rights' Legislations 
- Access to Goods

Education has significant impact on Labor Force and a Nation's overarching economy. Certain Industries, such as Space, recruits from the Labor Force that has Higher Education and higher to fill their employee requirements. As such, Education influences the following:
- Economic Productivity
- Research Speed
- Migration
- Political Participation
- Political Interests
- Social Mobility 
- Population Happiness.

A Population Centre with a large higher-education population may therefore develop substantially different economic and political characteristics from a Population Centre with predominantly primary-educated workers.











### Terrain Population Centres Demographic Attributes 
Each Population Centre possesses its own demographic attributes that significant contribute to FINISH. These attributes include:
- Cultures
- Population Types: The variety of Population Interest Groups in the Player's Nation. Population Types are influenced by Laws, Cultural Traits, Industries established within the Nation, Immigration, Migration, Wealth Distribution, Standard of Living, and Religion. A highly industrialised nation with high-income and advanced economy with significant welfare and higher education policies and a focus on Consumer Industries that produce plenty of Luxury Goods and Services Industries as well as a Constitutional Monarchy may produce more (GPT, THINK HIGHLY EDUCATED LIBERALS, TECHNOLOGISTS, TECHNBROS, EVEN MIDDLE CLASS HIPPES, FIND OR CREATE COLD WAR TYPE NAMES) which in turn influences the Political Groups of a Nation (how the population votes/what type of politics they want: whether democracy, authoritarian), Cultures dominate in a Nation (think that this highly rich liberal population may be more exceptionalist due to a cultural trait of saving the world or something), and Religion as well as overall Happiness and Migration Attraction. 



- Natural population growth:
- Migration:
- Immigration:
- Emigration: 
- Industries:
- Labour Force:
- Unemployment:
- GDP
- Standard of Living
- Tax
- Religions
- Primary Cultures:

Population Centres may therefore expand, contract, become damaged, be partially evacuated or experience significant demographic change during the simulation.
