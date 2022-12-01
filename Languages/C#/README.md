# C#

## Virtual vs Abstract
C#은 virtual 혹은 abstract 키워드가 있어야 override가 가능하다.
abstract는 parent에서 구조체를 제공해주지 않아 필수적으로 overrride를 해줘야 하며
virtual은 override를 할 지 말지를 선택할 수 있다.

물론 override하고자 하는 메서드가 private이라면 override가 되지 않으며
child에서 override 메서드의 엑세스 한정자를 바꿀 수 없다.