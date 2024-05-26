# Втора лабораториска вежба по Софтверско инженерство
## Aна Давидовска, бр. на индекс 223069
Control flow graph
![Screenshot 2024-05-26 203654](https://github.com/anadavid333/SI_2024_lab2_223069/assets/165026929/13fa9b3a-4823-4bfd-b44d-147b65c29394)

![Screenshot 2024-05-26 202516](https://github.com/anadavid333/SI_2024_lab2_223069/assets/165026929/1bb010a1-5410-433b-8288-3d5999c70395)








Цикломатска комплексност
M=E−N+2P,каде Е=37 N=29 =37-29+2=10
Цикломатската комплексност на овој код е 10 и истата ја добив преку горенаведената формула за пресметување. Буквата Е означува број на рабовите кои ги поврзуваат јазлите или таканаречени трансфери на контрола, ги имаме 37. Буквата N означува број на јазли во графот или секвенцијална група на изјави кои содржат само еден трансфер на контрола, и во нашиот случај изнесуваат 29. 
Тест случаи според критериумот Every Branch 

51-52 
52-53
53-92
52-56
56-58.1 
58.1-58.2
58.2-68 
68-87 
87-92 
90-92 
58.2-60 
60-63 
60-61 
61-63  
63-75 
75-92 
63-64 
64-65 
65-66.1 
66.1-66.2
66.2-67 
67-68
68-69 
69-92 
68-66.3
66.3-66.2
66.2-72 
72-73 
72-75
72-82 
82-83 
82-58.3
83-58.3
58.3-58.2
58.2-68
68-87 
87-92 
90-92

    void everyBranch() {

        RuntimeException exception;

      
        exception = assertThrows(RuntimeException.class, () -> SILab2.checkCart(null, 100));
        assertTrue(exception.getMessage().contains("allItems list can't be null!"));

       
        List<Item> emptyList = new ArrayList<>();
        assertTrue(SILab2.checkCart(emptyList, 100));

       
        List<Item> itemList1 = new ArrayList<>();
        itemList1.add(new Item(null, "12345", 50, 0.1f));
        assertTrue(SILab2.checkCart(itemList1, 100));

       
        List<Item> itemList2 = new ArrayList<>();
        itemList2.add(new Item("Item1", "12345", 50, 0.1f));
        assertTrue(SILab2.checkCart(itemList2, 100));

      
        List<Item> itemList3 = new ArrayList<>();
        itemList3.add(new Item("Item1", "12$45", 50, 0.1f));
        exception = assertThrows(RuntimeException.class, () -> SILab2.checkCart(itemList3, 100));
        assertTrue(exception.getMessage().contains("Invalid character in item barcode!"));

       
        List<Item> itemList4 = new ArrayList<>();
        itemList4.add(new Item("Item1", "12345", 50, 0.2f));
        assertTrue(SILab2.checkCart(itemList4, 100));

       
        List<Item> itemList5 = new ArrayList<>();
        itemList5.add(new Item("Item1", "12345", 50, 0));
        assertTrue(SILab2.checkCart(itemList5, 100));

        
        List<Item> itemList6 = new ArrayList<>();
        itemList6.add(new Item("Item1", "01234", 350, 0.1f));
        assertTrue(SILab2.checkCart(itemList6, 100));

        
        List<Item> itemList7 = new ArrayList<>();
        itemList7.add(new Item("Item1", "01234", 200, 0.1f));
        assertTrue(SILab2.checkCart(itemList7, 100));

       
        List<Item> itemList8 = new ArrayList<>();
        itemList8.add(new Item("Item1", "01234", 350, 0));
        assertTrue(SILab2.checkCart(itemList8, 100));

        
        List<Item> itemList9 = new ArrayList<>();
        itemList9.add(new Item("Item1", "12345", 350, 0.1f));
        assertTrue(SILab2.checkCart(itemList9, 100));

       
        List<Item> itemList10 = new ArrayList<>();
        itemList10.add(new Item("Item1", "12345", 50, 0.1f));
        assertTrue(SILab2.checkCart(itemList10, 100));

       
        List<Item> itemList11 = new ArrayList<>();
        itemList11.add(new Item("Item1", "12345", 150, 0.1f));
        assertFalse(SILab2.checkCart(itemList11, 100));
    }




Тест случаи според критериумот Multiple Condition

public class SILab2Test {

    @Test
    void testNullItemList() {
        assertThrows(RuntimeException.class, () -> SILab2.checkCart(null, 500));
    }

    @Test
    void testEmptyItemList() {
        List<Item> items = new ArrayList<>();
        assertTrue(SILab2.checkCart(items, 500));
    }

    @Test
    void testValidItemListNoDiscount() {
        Item item1 = new Item("Item1", "123456", 200, 0);
        Item item2 = new Item("Item2", "654321", 150, 0);
        List<Item> items = List.of(item1, item2);
        assertTrue(SILab2.checkCart(items, 500));
    }

    @Test
    void testValidItemListWithDiscount() {
        Item item1 = new Item("Item1", "123456", 200, 0.1f);
        Item item2 = new Item("Item2", "654321", 150, 0.2f);
        List<Item> items = List.of(item1, item2);
        assertTrue(SILab2.checkCart(items, 500));
    }

    @Test
    void testInvalidBarcode() {
        Item item = new Item("InvalidItem", "abc123", 200, 0.1f);
        List<Item> items = List.of(item);
        assertThrows(RuntimeException.class, () -> SILab2.checkCart(items, 500));
    }

    @Test
    void testDiscountPriceOver300BarcodeStartsWithZero() {
        Item item = new Item("SpecialItem", "012345", 400, 0.1f);
        List<Item> items = List.of(item);
        assertTrue(SILab2.checkCart(items, 500));
    }

    @Test
    void testDiscountPriceOver300BarcodeNotStartsWithZero() {
        Item item = new Item("SpecialItem", "123456", 400, 0.1f);
        List<Item> items = List.of(item);
        assertTrue(SILab2.checkCart(items, 500));
    }

    @Test
    void testDiscountPriceUnder300BarcodeStartsWithZero() {
        Item item = new Item("SpecialItem", "012345", 200, 0.1f);
        List<Item> items = List.of(item);
        assertFalse(SILab2.checkCart(items, 500));
    }

    @Test
    void testDiscountPriceUnder300BarcodeNotStartsWithZero() {
        Item item = new Item("SpecialItem", "123456", 200, 0.1f);
        List<Item> items = List.of(item);
        assertFalse(SILab2.checkCart(items, 500));
    }
}
Објаснување на Unit Tests
Unit тестовите  се дизајнирани за сеопфатна валидација на функционалноста на методот checkCart во класата SILab2. Секој тест-случај цели кон одредени сценарија за да се обезбеди правилно да се постапува и нормалното и исклуоците.

Тест за нула кај AllItems:
Потврдува дека методот исфрла RuntimeException со точната порака кога allItems е нула.

Teст за празна листа:
Проверува дали методот се враќа точно кога сите ставки се празни, бидејќи збирот (0) е помал или еднаков на уплатата.

Тест за item со null име:
Осигурува дека ставките со нула имиња се  „непознати“ .

Тест за валиден баркод:
Потврдува дека методот правилно ги обработува ставките со нумерички бар-кодови.

Тест за invalid баркод:
Потврдува дека методот фрла RuntimeException за ставки со ненумерички бар-кодови.

Тест за item со попуст поголем од 0:
Проверува дали методот правилно ја пресметува сумата кога артиклите имаат попусти.
 

Тест за item со цена над 300, попуст и баркод што започнува со „0“:
Потврдува дека дополнителниот попуст од 30 се применува правилно.


Тест за item со цена над 300 и без попуст, баркод што започнува со „0“:
Проверува да не се применува дополнителен попуст ако попустот е 0.


Тест за сума помала или еднаква на уплатата:
Потврдува дека методот се враќа точно кога вкупната сума е во рамките на уплатата.


Тест за сума поголема од уплатата:

Овие тестови ги покриваат сите можни branches и multiple conditions во рамките на методот, 
