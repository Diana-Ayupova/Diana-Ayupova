## Задание 1 

<p> Запрос, который выведет имя вида с наименьшим id соответствующим букве "М":<p> 
 <br/>SELECT species_name  
  <br/>FROM species  
  <br/>WHERE species_id = (SELECT MIN(species_id) FROM species)  
  
<p> Запрос, который выведет имя вида с количеством представителей более 1800 соответствующим букве "Б":<p> 
<br/>SELECT species_name 
<br/>FROM species 
<br/>WHERE species_amount > 1800
<p>Запрос, который выведет имя вида, начинающегося на "п" и относящегося к типу с type_id = 5 соответствующим букве "О":<p>
<br/>SELECT species_name 
<br/>FROM species 
<br/>WHERE species_name LIKE 'п%' AND type_id = 5
<p>Запрос, который выведет имя вида, заканчивающегося на "са" или количество представителей которого равно 5 соответствующим букве "В":<p>
<br/>SELECT species_name 
<br/>FROM species 
<br/>WHERE species_name LIKE '%са' OR species_amount = 5


## Задание 2 

<p>Запрос, который выведет имя вида, появившегося на учете в 2023 году соответствующий букве "Ы":<p>
<br/>SELECT species_name 
<br/>FROM species 
<br/>WHERE date_start >= '2023-01-01' AND date_start <= '2023-12-31'
<p>Запрос, который выведет названия отсутствующего (status = absent) вида, расположенного вместе с place_id = 3 соответствующий букве "С":<p>
<br/>SELECT species_name 
<br/>FROM species 
<br/>JOIN species_in_places ON species.species_id = species_in_places.species_id 
<br/>WHERE species_status = 'absent' AND place_id = 3
<p>Запрос, который выведет название вида, расположенного в доме и появившегося в мае, а также количество представителей вида. Название вида будет соответствовать букве "П":<p>
<br/>SELECT species.species_name, species.species_amount 
<br/>FROM species 
<br/>JOIN species_in_places ON species.species_id = species_in_places.species_id 
<br/>JOIN places ON species_in_places.place_id = places.place_id 
<br/>WHERE places.place_name = 'дом' AND EXTRACT(MONTH FROM species.date_start) = 5
<p>Запрос, который выведет название вида, состоящего из двух слов (содержит пробел) соответствующий знаку "!":<p>
<br/>SELECT species_name 
<br/>FROM species 
<br/>WHERE species_name LIKE '% %'


## Задание 3 


<p>Запрос, который выведет имя вида, появившегося с малышом в один день соответствующий букве "Ч":<p>
<br/>SELECT species.species_name 
<br/>FROM species 
<br/>JOIN species AS baby_species ON species.date_start = baby_species.date_start AND species.species_id != baby_species.species_id 
<br/>WHERE baby_species.species_name = 'малыш'
<p>Запрос, который выведет название вида, расположенного в здании с наибольшей площадью соответствующий букве "Ж":<p>
<br/>SELECT species.species_name 
<br/>FROM species 
<br/>JOIN species_in_places ON species.species_id = species_in_places.species_id 
<br/>JOIN places ON species_in_places.place_id = places.place_id 
<br/>ORDER BY places.place_size DESC 
<br/>LIMIT 1
<p>Запрос, который найдет название вида, относящегося к 5-й по численности группе проживающей дома соответствующий букве "Ш":<p>
<br/>SELECT species.species_name 
<br/>FROM species 
<br/>JOIN species_type ON species.type_id = species_type.type_id 
<br/>WHERE species_type.type_id IN (
    <br/>SELECT species_type.type_id
    <br/>FROM species_type 
    <br/>JOIN species ON species.type_id = species_type.type_id 
    <br/>GROUP BY species_type.type_id 
    <br/>ORDER BY COUNT(*) DESC 
    <br/>OFFSET 4
)
<p>Запрос, который выведет сказочный вид (статус fairy), не расположенный ни в одном месте соответствующий букве "Т":<p>
<br/>SELECT species.species_name 
<br/>FROM species 
<br/>WHERE species.species_status = 'fairy' 
AND species.species_id NOT IN (
    <br/>SELECT species_in_places.species_id 
    <br/>FROM species_in_places
)
