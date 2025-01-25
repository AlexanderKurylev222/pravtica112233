Задание 3
https://www.figma.com/design/zvDbNXyl26qpUqiPD5kJka/Untitled?node-id=0-1&p=f&t=GCEWGOwCZ956prlV-0

Задание 2 
# views.py (Django) 
from django.http import JsonResponse 
from django.views.decorators.csrf import csrf_exempt 
from .models import Order, OrderItem 
@csrf_exempt 

def create_order(request): 
    if request.method == 'POST': 
        try: 
            data = json.loads(request.body) 
            # Создание заказа 
            order = Order.objects.create(user=data['user'], status='created', total_price=data['total_price']) 
            # Добавление товаров в заказ 
            for item in data['items']: 
                OrderItem.objects.create(order=order, product=item['product'], quantity=item['quantity'], price=item['price']) 
            return JsonResponse({'status': 'success', 'message': 'Order created successfully', 'order_id': order.id}, status=201) 
        except Exception as e: 
            return JsonResponse({'status': 'error', 'message': str(e)}, status=400) 

 

Задание 4  
GET Запрос 
{ 
  "order_id": "12345", 
  "status": "Новый", 
  "items": [ 

    { "item_id": "678", "name": "Товар 1", "quantity": 1, "price": 100 }, 
    { "item_id": "679", "name": "Товар 2", "quantity": 2, "price": 200 } 

  ], 
  "total": 500, 
  "delivery_address": "г. Москва, ул. Примерная, д. 1", 
  "contact_info": "Иван Иванов, +79991234567", 
  "comment": "Доставить до 18:00" 
} 

 

PUT Запрос 
{ 

  "items": [ 

    { "item_id": "678", "quantity": 2 }, 
    { "item_id": "680", "quantity": 1 } 
    
    ], 
  "delivery_address": "г. Москва, ул. Новая, д. 2", 
  "contact_info": "Петр Петров, +79998765432", 
  "comment": "Оставить у двери" 
} 

 
Задание 5
1. Вывести покупателей с количеством осуществленных покупок:

SELECT 
    Покупатели.Имя,
    Покупатели.Фамилия,
    COUNT(Покупки.Идентификатор) AS Количество_покупок
FROM 
    Покупатели
JOIN 
    Покупки ON Покупатели.Идентификатор = Покупки.Ключ_покупателя
GROUP BY 
    Покупатели.Идентификатор, Покупатели.Имя, Покупатели.Фамилия;

   
3. Общую стоимость товаров для каждого покупателя и отсортировать результат в порядке убывания:

SELECT 
    Покупатели.Имя,
    Покупатели.Фамилия,
    SUM(Товары.Стоимость) AS Общая_стоимость
FROM 
    Покупатели
JOIN 
    Покупки ON Покупатели.Идентификатор = Покупки.Ключ_покупателя
JOIN 
    Товары ON Покупки.Ключ_товара = Товары.Идентификатор
GROUP BY 
    Покупатели.Идентификатор, Покупатели.Имя, Покупатели.Фамилия
ORDER BY 
    Общая_стоимость DESC;

   
5. Получить покупателей, купивших только один товар:

SELECT 
    Покупатели.Имя,
    Покупатели.Фамилия
FROM 
    Покупатели
JOIN 
    Покупки ON Покупатели.Идентификатор = Покупки.Ключ_покупателя
GROUP BY 
    Покупатели.Идентификатор, Покупатели.Имя, Покупатели.Фамилия
HAVING 
    COUNT(Покупки.Идентификатор) = 1;
