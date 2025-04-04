# Задание 1 (обязательное)

Шаг 1. Создайте проект на базе *Maven*.

Шаг 2. Добавьте в проект *JUnit Jupiter & Surefire Plugin*.

Шаг 3. Создайте сервисный класс со следующим исходным кодом:

public class BonusService {
  public long calculate(long amount, boolean registered) {
    int percent = registered ? 3 : 1;
    long bonus = amount * percent / 100;
    long limit = 500;
    if (bonus > limit) {
      bonus = limit;
    }
    return bonus;
  }
  
}

Шаг 4. Создайте тестовый класс со следующим исходным кодом:

import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.Assertions;

public class BonusServiceTest {

  @Test
  void shouldCalculateForRegisteredAndUnderLimit() {
    BonusService service = new BonusService();

    // подготавливаем данные:
    long amount = 1000;
    boolean registered = true;
    long expected = 30;

    // вызываем целевой метод:
    long actual = service.calculate(amount, registered);

    // производим проверку (сравниваем ожидаемый и фактический):
    Assertions.assertEquals(expected, actual);
  }

  @Test
  void shouldCalculateForRegisteredAndOverLimit() {
    BonusService service = new BonusService();

    // подготавливаем данные:
    long amount = 1_000_000;
    boolean registered = true;
    long expected = 500;

    // вызываем целевой метод:
    long actual = service.calculate(amount, registered);

    // производим проверку (сравниваем ожидаемый и фактический):
    Assertions.assertEquals(expected, actual);
  }
  
}

Шаг 5. Запустите тесты через **mvn clean test**, убедитесь, что они запускаются и проходят.

Шаг 6. Проведите поверхностный тест-дизайн сервисного класса, допишите как минимум два недостающих и прямо напрашивающихся теста.

Шаг 7. Убедитесь, что тесты запускаются и проходят.

# Задание 2 (выполнять не обязательно)

Создадим подобный дефект. Перейдите в класс BonusService и переименуйте переменную в нём с percent на Percent. Убедитесь, что код компилится, тесты проходят.

Добавьте в **pom.xml** в плагины следующую настройку:

              <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-checkstyle-plugin</artifactId>
                <version>3.2.1</version>
                <configuration>
                    <checkstyleRules>
                        <module name="Checker">
                            <module name="TreeWalker">
                                <module name="LocalVariableName"/>
                            </module>
                        </module>
                    </checkstyleRules>
                </configuration>
                <executions>
                    <execution>
                        <id>validate</id>
                        <phase>validate</phase>
                        <goals>
                            <goal>check</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
Это подключит maven-checkstyle-plugin к вашему проекту и сконфигурирует проверку на правило LocalVariableName, которое проверяет правильность именований локальных переменных, а саму проверку (check в executions) будет запускать на ранней файзе validate ещё до тестов.

Запустите mvn clean test. Сборка должна упасть, так как плагин должен найти эту ошибку именования. Откройте полные логи сборки, кликнув на верхний пункт в левом нижнем окне (по умолчанию будет выделен самый нижний, как на скриншоте ниже), прокручивая окно логов сборки сверху вниз, найдите первое упоминания слова ERROR (с англ. «ошибка»), убедитесь, что в этой строке идёт речь про неправильное именование переменной Percent:

![image](https://user-images.githubusercontent.com/53707586/212552797-162c2265-66b0-4206-b6a3-1d3f0de55568.png)

Сделайте коммит с комментарием Rename percent to Percent и запушьте изменения на GitHub. Создайте баг-репорт на основе GitHub Issues. В шагах воспроизведения укажите запуск mvn clean test, в ожидаемом результате успешное прохождение сборки, в фактическом — падение из-за неправильного именования переменной Percent.

Добавьте в баг-репорт ещё один раздел для логов сборки. Синтаксис красивого его оформления будет следующим. Обратите внимание на пробелы и пустые строки, это важно, чтобы всё сработало:

![image](https://user-images.githubusercontent.com/53707586/212553230-ad39ce10-9eb8-4fa1-a60d-2530a5137d62.png)

После создания баг-репорта переключитесь на IDEA, исправьте дефект (переименуйте обратно на percent), убедитесь, что сборка проходит успешно, сделайте коммит с сообщением **Fix #1** и пуш, где 1 — это номер баг-репорта, который будет закрыт автоматически при пуше этого коммита в основную ветку.
