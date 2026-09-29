# Industries 
Industries are key economic sectors that play a crucial role in boosting a Nation’s Productivity and Average Gross Salary, both of which directly impact economic growth. Industries are established with Strategic Resources. Certain Industries provide access to the production of specific Military Armaments, Civilian Infrastructure, allows the natural repairing of damaged Terrain Objects, Population Centres, and FINISH.

Each industry is established and maintained using its parent Strategic Resource, making resource management vital for industrial growth. The Player establishes an Industry by navigating to their Economy Tab. Established Industries are not directly controlled by the Player, but by a Company AI native to the Player's Nation. However the output of an Industry is managed by the Player for Trading or Military Production. This Company is controlled by an Executive Character Type who provides modifiers to an Industry depending on their Statistics, Traits, Interest (from the same Population Interest List) and Political Affiliation. 

On the World Map, Industries are geographic Areas created using the Area Mapping Primitive in the MapMaker and visually represented by an Industrial Bitmap. When the Player selects this INSERT on the Worldmap, they are presented with an Interface that shows the Industry's:

- Company Name & Logo (the company logo of the Industry, these will be custom to the nation, handmade to fit the specific majority culture)
- Senior Executive (the character who controls the company and the modifiers he/she provides; the Player should have an option to fire the Executive, but successfully firing will be dependent on the simulation engine and if the Executive is popular or not )
- Employment Rate
- Laborforce Composition (Population Types; and Worker Happiness)
- Average Gross Salary
- Global Subsidiaries (displays the global subsidiaries this particular Company has for the Industry; Global Subsidiaries occur when the Player has a specific Trade Agreement with a foreign nation, and the Player is able to establish the same Industry in a foreign nation; the income from that foreign nation will be decided in the agreement)
- Input (The Strategic Resource it is consuming daily, the higher the productivity, the more Resource used; the Player can demand an industry produces faster which boosts Output, which can be accepted by the Senior Executive AI but they may demand a capital injection; higher production utilises more Input, and boosts Output)
- Output (The Goods it is creating daily and where those goods are going, whether to foreign nations, the population of its parent nation, or the Military in the case of Industries that make weaponry, or the Services it is providing to a Nation which can adds modifiers to the Population, Government, and so on; this should be numerical, giving a clear indication of how much is being made a day; any modifiers impacting the Output should also be shown; what goods an Industry Outputs is dependent on the Traits and Characteristics of the Senior Executive, but the Player can demand specific goods take precedence)
- Revenue (this is how much money the industry is making via its outputs and from whom, as well as costs; this should also be a historical view in the UI so Players can see impact)
- Total Expenditure (this is how much the industry is spending on paying its labour force, producing goods, and paying corporate tax; how much it pays its laborforce is dependent on the Education it demands from the Population, as well as relevant Civil Rights legislations; how much it pays in corporate tax is set by Economy legislations via the Government System; this should all be shown to the Player)
- Foreign Investment (this is how much capital the industry is receiving from a foreign nation; foreign investment accessibility is dependent on diplomacy established with other Nations; foreign investment subsidies the Industry, and gives the foreign investor 
- Sanctions (this shows the sanctions being placed on the Industry by adversaries; sanctions have a major impact on an industry and the Player can apply them and they can be applied against the Player's Industries by AI Nations)
- Operating Income (this is how much the Industry is making, which can be a negative or positive; if the operating income is negative for a prolonged period, the industry will crash and require a bailout; likewise, the Senior Executive AI will automatically inflate how much it charges for goods, production, and so on based on FINISH; the Player can subsidise the industry at any point and the Senior Executive AI will reach out to the Player to subsidise it when it demands so)

## All Industries

### Transportation
| Industry       | Resource Input | Establishment Cost | Base Input Rate Per Day | Education Required | Base Gross Salary |
| :------------- | :------------- | :----------------- | :---------------------- | :----------------- | :---------------- |
| Transportation | Petroleum      | 10,000 Barrels     | FILL                    | FILL               | FILL              |

Output for this Industry as as follows:

| Good                  | Consumer   | Base Cost | Traded Quantity | Standard of Living Modifier | Happiness Modifier | Interest Alignment Modifier |
| :-------------------- | :--------- | :-------- | :-------------- | :-------------------------- | :----------------- | :---------------------- |
| Local Bus Services    | Population | 15        | N/A             | FILL                        | FILL               | Social Welfare (INSERT VALUE)          |
| Domestic Flights      | Population | 200       | N/A             | FILL                        | FILL               |                         |
| International Flights | Population | 500       | N/A             | FILL                        | FILL               |                         |
| Passenger Rail        | Population | 40        | N/A             |                             |                    |                         |





## Company
