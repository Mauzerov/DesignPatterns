# Wzorce Projektowe

---
>## Czym są?
> Są to uniwersalne, sprawdzone w praktyce rozwiązania często pojawiających się problemów projektowych
> Pokazuje powiązania i zależności pomiędzy klasami oraz obiektami i ułatwia tworzenie, modyfikację oraz utrzymanie kodu źródłowego

>## Na czym polegają?
> Zamiast skupiać się na funkcjonowaniu poszczególnych elementów, wzorce projektowe stanowią abstrakcyjny opis zależności pomiędzy klasami,
> co w efekcie wprowadza pewną standaryzację kodu oraz zwiększa jego zrozumiałość, wydajność i niezawodność.
> Wzorce projektowe mogą przyspieszyć proces rozwoju oprogramowania przez dostarczenie wypróbowanych rozwiązań dla problemów, które mogą nie być oczywiste na początku procesu projektowego.

>## Przykładowe Wzorce
> - Strategy Pattern
> - Decorator Pattern
> - Adapter Pattern
> - Facade Pattern
> - Command Pattern
> - Template Method
> - Builder Pattern

>## Zastosowania
> > ### Strategy Pattern
> > Wzorzec projektowy **Strategy Pattern** określa rodzinę funkcjonalności i umożliwia zamienne ich stosowanie.
> Strategie nie dziedziczą po żadnej konkretnej klasie, a jedynie implementują wspólny interfejs.
> > <br>Można go wykożystać do implementacji np. promocji:
> > ```kt
> > interface Promotion {
> >     fun calculate(sum: Double): Double
> >     val name: String
> > }
> > 
> > class ChristmasPromotion : Promotion {
> >     override val name = "Christmas Promotion"
> >     override fun calculate(sum: Double): Double {
> >         return sum * 0.8
> >     }
> > }
> > 
> > class NoPromotion : Promotion {
> >     override val name = "No Promotion"
> >     override fun calculate(sum: Double): Double {   
> >         return sum
> >     }
> > }
> > 
> > class SpecialPromotion : Promotion { 
> >     override val name = "Special Promotion"
> >     override fun calculate(sum: Double): Double {
> >         return when {
> >             sum > 40 -> sum * 0.75
> >             sum > 30 -> sum * 0.85
> >             sum > 20 -> sum * 0.95
> >             else -> sum
> >         }
> >     }
> > }
> > ```
> 
> > ### Command Pattern
> > Wzorzec **Command** polega na opakowaniu żądania w konkretny obiekt, posiadający wszystkie informacje niezbędne do wykonania swojego zadania
> > <br>Obiekt żądania, oprócz standardowej metody `execute()` może zawierać metodę w stylu `undo()` czyli cofnięcie wprowadzanych przez żądanie zmian. Praktycznie wszystkie programy graficzne czy edytory tekstu mają taką opcję. Przechowują każdą zmianę w formie Polecenia w jakimś bufferze.
> > ```kotlin
> > abstract class Command(val receiver: Receiver) {
> >     abstract fun execute()
> > }
> > // konketne polecenie przyjmujące Receiver w konstruktorze
> > class ConcreteCommand(receiver: Receiver) : Command(receiver) {
> >     override fun execute() {
> >         println("executing ConcreteCommand")
> >         // wywołanie odpowiedniej akcji na odbiorcy
> >         receiver.action()
> >     }
> > }
> > // rzeczywisty "wykonawca" akcji, znający szczegóły implementacji
> > class Receiver {
> >     fun action() { }
> > }
> > // obiekt wykonujący polecenie, które jest ustawiane setterem
> > // np. przycisk wywołujący polecenie po kliknięciu
> > class Invoker {
> >     private lateinit var command: Command
> >
> >     fun setCommand(command: Command) {
> >         this.command = command
> >     }
> >
> >     fun performAction() {
> >         this.command.execute()
> >     }
> > }
> > // klasa łącząca odbiorcę, polecenie i obiekt wywołujący polecenie
> > class Client(invoker: Invoker) {
> >     private val receiver = Receiver()
> >     init {
> >         val concreteCommand = ConcreteCommand(receiver)
> >         invoker.setCommand(concreteCommand)
> >     }
> > }
> > fun main() {
> >     val invoker = Invoker()
> >     Client(invoker)
> >     invoker.performAction()
> > }
> >```
> 
> > ### Builder Pattern
> > Wzorzec Builder służy do uproszczenia tworzenia złożonych obiektów ze skomplikowaną logiką budowania lub z wieloma argumentami w konstruktorze.
> > Dzięki zastosowaniu tego wzorca obiekt łatwo może być niemutowalny, bo dostajemy taki obiekt, jaki ustawiliśmy przez Buildera
> > ```kt
> > class Computer private constructor() {
> >    lateinit var HDD: String
> >        private set
> >    lateinit var RAM: String
> >        private set
> >    var isGraphicsCardEnabled by Delegates.notNull<Boolean>()
> >        private set
> >    var isBluetoothEnabled by Delegates.notNull<Boolean>()
> >        private set
> >
> >    class ComputerBuilder() {
> >        private val computer = Computer()
> >
> >        fun setHDD(hdd: String) : ComputerBuilder = apply {
> >            computer.HDD = hdd
> >        }
> >
> >        fun setRAM(ram: String) : ComputerBuilder = apply {
> >            computer.RAM = ram
> >        }
> >
> >        fun setGraphicsCardEnabled(enabled: Boolean): ComputerBuilder = apply {
> >            computer.isGraphicsCardEnabled = enabled
> >        }
> >
> >        fun setBluetoothEnabled(enabled: Boolean): ComputerBuilder = apply {
> >            computer.isBluetoothEnabled = enabled
> >        }
> >
> >        fun build(): Computer {
> >            return computer
> >        }
> >    }
> >}
> > ```
