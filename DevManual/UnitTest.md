## **단위 테스트**

**MSTest란?**
class 작성시 작성한 class의 기능만 간단하게 검증 할 수 있는 VisualStudio(.net)의 테스트 라이브러리
***
![1.png](../DCOM/DevManual/UnitTest/1.png)

**Edit 요령**
1. 솔루션탐색기 최상단(R Click) - 추가(D) - 새 프로젝트(N) - MSTest 테스트 프로젝트
2. 참조에 기존프로젝트 추가.
3. 단위테스트할 .cs의 네임스페이스 선언.
4. [TestMethod]어트리뷰트 선언부 밑에 함수 작성.

TestMethod의 요소는 3개의 Part로 구성된다.
Arrange: 멤버변수 혹은. 테스트 하기 위한 파라메터들을 준비하는 곳
Act: 파라메터를 아규먼트로 실행, Buisness로직에 의해 변경된다.
Assert: 예외를 Throw하는 조건들을 정의하는곳. ( Fail과 Success가 결정되는 구간) 

UnitTest.cs 작성 예시

```CSharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using System;
using DCOM.Managers;
using System.Collections.Generic;
using DCOM.Information;
using System.Linq;

namespace HY_UTest01
{
    [TestClass]
    public class UnitTest1
    {
        [TestMethod]
        public void TestMethod_connection()
        {
            // Arrange
            var _manager = new DatabaseManager(new List<Session>() 
            { 
                new Session()
                { 
                    Database= DBMS.MongoDB,
                    Host = "ICMS1",
                    Port = "27017",
                    Id = " ",
                    Password = " ",
                } ,
                new Session()
                {
                    Database= DBMS.MongoDB,
                    Host = "ICMS2",
                    Port = "27017",
                    Id = " ",
                    Password = " ",
                }
            });

            // Act
            _manager.ConnectAll();

            // Assert
            Assert.IsFalse(_manager.SessionInfos.Select(s => s.Status).ToList().Contains("Fail"));
        }
        [TestMethod]
        public void TestMethod_Read() 
        {
            var _manager = new DatabaseManager(new List<Session>()
            {
                // Arrange
                new Session()
                {
                    Database= DBMS.MongoDB,
                    Host = "ICMS1",
                    Port = "27017",
                    Id = " ",
                    Password = " ",
                } ,
                new Session()
                {
                    Database= DBMS.MongoDB,
                    Host = "ICMS2",
                    Port = "27017",
                    Id = " ",
                    Password = " ",
                }
            });

            // Act
            _manager.ConnectAll();

            //var docs = _manager.Select("ICMS", "Logs", 0, 20, false, "createAt");
            var ff = _manager.GetSelecedCount("ICMS", "Settings");

            // Assert
            Assert.IsFalse(ff == 0);
        }
        [TestMethod]
        public void TestMethod_GetHost() 
        {
            // Arrange
            var _manager = new DatabaseManager(new List<Session>()
            {
                new Session()
                {
                    Database= DBMS.MongoDB,
                    Host = "ICMS1",
                    Port = "27017",
                    Id = " ",
                    Password = " ",
                } ,
                new Session()
                {
                    Database= DBMS.MongoDB,
                    Host = "ICMS2",
                    Port = "27017",
                    Id = " ",
                    Password = " ",
                }
            });

            // Act
            _manager.ConnectAll();
            _manager.GetThisHost(out string host, out int port);

            // Assert
            Assert.IsFalse(string.IsNullOrEmpty(host));
            Assert.AreNotEqual(0, port);

        }
        [TestMethod]
        public void TestMethod_Success() 
        { 
	        //성공
        }

        [TestMethod]
        public void TestMethod_Fail() 
        {
	        //실패 
            Assert.Fail();
        }
        [TestMethod]
        public void TestMethod_Inconclusive() 
        {
	        // 결론에 이르지 못하는 애매함.
            Assert.Inconclusive();
        }
    }
}

```
**참고**
[DatabaseManager.cs](https://github.com/HANLAIMS/RD136-SW-DCOM/blob/main/DCOM/Managers/DatabaseManager.cs)
***
![2.png](../DCOM/DevManual/UnitTest/2.png)

**테스트 방법**
 1. 상단메뉴 - 테스트(s)
 2. 모든테스트 실행 : [TestMethod]어트리뷰트의 모든 메서드 실행
 3. 모든테스트 디버깅: 중단점 잡고 디버그 가능
***

**특 장점**
1. 테스트를 진행하기 위한 UI를 만들 필요 없다.
2. 강력한 라이브러리 기능으로 코드와 경과등을 쉽고 한눈에 빠르게 파악, 
3. 여러가지 테스트를 병행하여 진행 할 수가 있다. 
4. 기능 추가시 관련 기능만 빠르게 추가하여 테스트 가능.
***

> Written with [StackEdit](https://stackedit.io/).
