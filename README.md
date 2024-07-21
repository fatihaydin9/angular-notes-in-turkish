# Angular 18 Çalışma Notları

# İçindekiler

- [1. Angular CLI (Command-Line Interface)](#1-angular-cli-command-line-interface)
- [2. Temel Dosya Yapısı](#2-temel-dosya-yapısı)
- [3. Angular Çalışma Ortamı: Eklenti ve Snippet Kurulumları](#3-angular-çalışma-ortamı--eklenti-ve-snippet-kurulumları)
- [4. TypeScript Temelleri](#4-typescript-temelleri)
  - [4.1. Type Inference](#41-type-inference)
  - [4.2. Interfaces](#42-interfaces)
  - [4.3. TypeScript Türleri](#43-typescript-türleri)
  - [4.4. TypeScript’de Gelişmiş Türler](#44-typescript-de-gelişmiş-türler)
  - [4.5. Decorators in TypeScript](#4-5-decorators-in-typescript)
- [5. Angular Komponent Yapısı](#5-angular-komponent-yapısı)
  - [5.1. Angular’da Komponent Oluşturma](#51-angular’da-komponent-oluşturma)
  - [5.2. Angular’da Komponent Ömrü Metodları (Component Lifecycle Hooks)](#52-angular’da-komponent-ömrü-metodları-component-lifecycle-hooks)
- [6. Angular Servis Mimarisi](#6-angular-servis-mimarisi)
- [7. Decorator Pattern & Decorators in Angular](#7-decorator-pattern--decorators-in-angular)
  - [7.1. Decorator Pattern Fundamentals](#71-decorator-pattern-fundamentals)
  - [7.2. Angular'da @ViewChild, @Input, @Output Dekorasyonları](#72-angularda-viewchild-input-output-dekorasyonları)


## 1. Angular CLI (Command-Line Interface)

İlk önce homebrew kurulumu olmalı : 

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Sonrasında node.js ‘i kurmalıyız : 

```bash
brew install node
```

Angular CLI kurulumu : 

```bash
npm install -g @angular/cli
```

Angular CLI versiyon bilgisi öğrenme :

```bash
ng version
```

<aside>
💡 Dependencies yani bağımlılıklarımızın bulunduğu package.json dosyasında scripts kısmı npm start, npm build vs. komutları çalıştırıldığında Anguların bu komutları nasıl karşılayacağı yer almaktadır.

</aside>

<aside>
💡 Ayrıca, package.json projeye ait bağımlılıkları, metadata dediğimiz proje bilgilerini, script ayarlarını içerirken ; package.lock.json node_modules altındaki tüm bağımlılıkları içerir ve npm install komutuyla indirilecek gerekli tüm kurulumu yapılacak modullerin kesin bir tanımını içerir.

</aside>

Yeni bir Angular projesi oluşturmak için  (MacOS ‘da yönetici olarak çalıştırmak gerekebiliyor): 

```bash
(sudo) ng new angular-app
```

## 2. Temel Dosya Yapısı

- **node_modules :** Tüm paket bağımlılıklarını (dependencies) kütüphaneleri içeren klasördür. Binlerce alt dosyadan oluştuğundan, projemizi bir yere yedeklerken bu klasörü silebiliriz, zaten package-lock.json içerisinde tüm bağımlılıkların listesi bulunduğundan **`npm install`**  diyerek bağımlılıkları kısa bir sürede indirebiliriz.

---

- **public :** Yaygın adıyla assets, uygulamaların statik klasörlerini içerir. Örnek bir public (assets) klasörünü şu şekilde tanımlayabiliriz :
    
    Genel olarak statik bir biçimde resimleri (logo, arkaplan resimleri vb), bunun yanı sıra fontları, veya HTTP Request göndermeden önce örnek bir verinin json halini bu statik klasöre ekleyebiliriz.
    
    Default olarak favicon.ico dosyası buraya tanımlı olarak gelmektedir. Bu klasörü statik amaçlı kullanmalıyız. Dilersek ismini assets yapabiliriz, daha mantıklı duruyor.
    
---

- **src :** Burada ise source yani kaynak dosyalarımız bulunmaktadır. **`/app`** klasörü içerisinde uygulamamızın ilk komponenti yer alıyor. Burada -
    - app.component.html : Komponentin html yapısını içerir.
    - app.component.scss (css) : Burada ise tercihe göre scss veya css ile komponente ait tasarımı geliştirebiliyoruz.
    - app.component.spec.ts : Burada ise test kodları yazılabiliyor, front-end testleri ileride araştırılacaktır.
    - app.component.ts : Komponentimizin en temel ve nihai bilgilerini içerir.
    - app.config.ts ve app.routes.ts : Uygulamanın routing ve uygulama ayarlarının tanımlanmasını sağlar.

<aside>
💡 Not : **`src/`** klasörünün altındaki **`index.html`** **`main.ts`** ve **** **`styles.(s)css`** dosyaları global ve tüm sınıfları içeren genel kullanımlar için oluşturulmuş klasörlerdir.

</aside>

---

- **.gitignore :** Burada ise Git VCS (Version Control System) için “görmezden gelinmesi gereken” dosyaların uzantılarını belirtiyoruz. Aslında bu dosyaların bazıları kritiktir ve uygulamaya ait doğrudan bilgileri içerirler, bu nedenle bu dosyaların versiyon kontrol sistemlerine pushlanmaması gerekmektedir.

---

- **angular.json :** Temel olarak şu görevleri yerine getirir :
1. **Proje Yapılandırması**: Projenizin adı, dizin yapısı ve giriş noktası gibi bilgileri içerir.
2. **Yapı ve Derleme Ayarları**: Hangi dosyaların derleneceği, derleme seçenekleri, optimize edilmiş üretim yapılarını ve geliştirme ortamı ayarlarını belirler.
3. **Servis Çalıştırma**: Uygulamanızın geliştirme sunucusunu nasıl başlatacağınızı ve hangi portta çalışacağını tanımlar.
4. **Test Ayarları**: Testlerin nasıl çalıştırılacağına dair ayarları içerir.
5. **Mimari ve Yapılandırma Ayarları**: Projeye eklenen kütüphaneler, varlıklar, stil dosyaları ve global ayarlar gibi bilgileri barındırır.

Ayrıca hangi ortamda hangi dosyaların replace edileceği (transform) ayarları, uygulamaya ait önemli ayarları barındırır (.NET ‘deki appconfig.json benzeri bir yapıdır, her ne kadar doğrudan karşılığı olmasa da).

Örneğin bir ortama göre build yapmak isteseydik :

```bash
	ng build --configuration production 
```

Örnek bir angular.json dosyası şu şekildedir : 

```json
"projects": {
  "your-project-name": {
    "projectType": "application",
    "schematics": {},
    "root": "",
    "sourceRoot": "src",
    "prefix": "app",
    "architect": {
      "build": {
        "builder": "@angular-devkit/build-angular:browser",
        "options": {
          "outputPath": "dist/your-project-name",
          "index": "src/index.html",
          "main": "src/main.ts",
          "polyfills": "src/polyfills.ts",
          "tsConfig": "tsconfig.app.json",
          "assets": [
            "src/favicon.ico",
            "src/assets"
          ],
          "styles": [
            "src/styles.css"
          ],
          "scripts": []
        },
        "configurations": {
          "production": {
            "fileReplacements": [
              {
                "replace": "src/environments/environment.ts",
                "with": "src/environments/environment.prod.ts"
              }
            ],
            "optimization": true,
            "outputHashing": "all",
            "sourceMap": false,
            "extractCss": true,
            "namedChunks": false,
            "aot": true,
            "extractLicenses": true,
            "vendorChunk": false,
            "buildOptimizer": true,
            "budgets": [
              {
                "type": "initial",
                "maximumWarning": "2mb",
                "maximumError": "5mb"
              }
            ]
          }
        }
      },
      "serve": {
        "builder": "@angular-devkit/build-angular:dev-server",
        "options": {
          "browserTarget": "your-project-name:build"
        },
        "configurations": {
          "production": {
            "browserTarget": "your-project-name:build:production"
          }
        }
      },
      "extract-i18n": {
        "builder": "@angular-devkit/build-angular:extract-i18n",
        "options": {
          "browserTarget": "your-project-name:build"
        }
      },
      "test": {
        "builder": "@angular-devkit/build-angular:karma",
        "options": {
          "main": "src/test.ts",
          "polyfills": "src/polyfills.ts",
          "tsConfig": "tsconfig.spec.json",
          "karmaConfig": "src/karma.conf.js",
          "assets": [
            "src/favicon.ico",
            "src/assets"
          ],
          "styles": [
            "src/styles.css"
          ],
          "scripts": []
        }
      },
      "lint": {
        "builder": "@angular-devkit/build-angular:tslint",
        "options": {
          "tsConfig": [
            "tsconfig.app.json",
            "tsconfig.spec.json",
            "e2e/tsconfig.json"
          ],
          "exclude": [
            "**/node_modules/**"
          ]
        }
      }
    }
  }
}

```

---

- **package.json :** Bu dosya uygulamaya ait metadata dediğimiz temel bilgileri (uygulama adı, versiyonu, uygulamaya has bağımlılıkları) içeren ; bunun yanı-sıra scripts içerisinde npm komutlarına karşılık gelen angular komutlarını da belirttiğimiz bir json ayar dosyasıdır. Örneğin aşağıdaki örnekten de anlayacağımız üzere, **`npm start`** komutu ‘nun Angular tarafındaki karşılığı **`ng-serve`** olacaktır.

```json
  "scripts": {
    "ng": "ng",
    "start": "ng serve",
    "build": "ng build",
    "watch": "ng build --watch --configuration development",
    "test": "ng test"
  }
```

- **package-lock.json :** Burada npm install dediğimizde node_modules klasörü içerisine indirilecek tüm bağımlılıkları içerir. Bu nedenle node_modules klasörü silinse bile, package-lock.json tam bir bağımlılık listesini tuttuğundan herhangi bir kayıp yaşanmaz. Bu nedenle Angular uygulama klasörümüzü bir yerden bir yere taşırken node_modules klasörünü silip taşımalıyız ; yüzlerce bağımlılık ve binlerce alt klasörün taşınması hayli uzun sürecektir.

---

- **README file :** Bu dosya klasik olarak açık kaynak yazılan uygulamalarda ; uygulamanın temel içeriği, lokalde nasıl çalıştırılacağı ve mimarisi gibi temel bileşenleri içerir.

---

- **tsconfig.app.json :** Bu dosya, uygulamanın TypeScript yapılandırma ayarlarını içerir. Genellikle uygulamanın hangi dosyalarının derleneceğini ve hangi ayarlarla derleneceğini belirtir.

---

- **tsconfig.json :** Bu, tüm projenin genel TypeScript yapılandırma dosyasıdır. İçinde, projenin derleme seçenekleri, dahil edilecek dosyalar ve dışlanacak dosyalar gibi ayarlar bulunur. Diğer tsconfig dosyaları, bu dosyadaki ayarları miras alabilir.

---

- **tsconfig.spec.json :** Bu dosya, testler için TypeScript yapılandırma ayarlarını içerir. Test dosyalarının derlenmesi ve çalıştırılması için gereken özel ayarları belirtir.

---

## 3. Angular Çalışma Ortamı : Eklenti ve Snippet Kurulumları

Angular ‘da kullanacağımız eklentiler (vsCode Extensions): 

1. **`Angular Language Service :`** Angular template için autocomplete özelliklerini içerir.
2. **`Angular Snippets :`** Angular ile TypeScript yazımında code-snippet kısayolu sağlar.
3. **`Prettier :`** Yazdığımız kodların belirli bir standartta formatlanmasını sağlar (bknz: .editorconfig) 
4. **`Auto Rename Tag :`** Yazdığımız ilk tagın otomatik olarak kapanış tagını oluşturmada kullanılır. Ayrıca tag için düzenleme yaptığınızda, kapanış tagı da otomatik olarak güncellenir.

Prettier kod formatlayıcısı kullanmak için şu kodları **`editorsettings.json`** ‘a eklemeliyiz (VSCode)

```json
"editor.defaultFormatter": "esbenp.prettier-vscode",
"[javascript]": {
  "editor.defaultFormatter": "esbenp.prettier-vscode"
},
"[typescript]": {
  "editor.defaultFormatter": "esbenp.prettier-vscode"
},
"[json]": {
  "editor.defaultFormatter": "esbenp.prettier-vscode"
},
"[html]": {
  "editor.defaultFormatter": "esbenp.prettier-vscode"
},
"[css]": {
  "editor.defaultFormatter": "esbenp.prettier-vscode"
},
"[markdown]": {
  "editor.defaultFormatter": "esbenp.prettier-vscode"
}
```

Dosyayı kaydettiğimizde formatlanması için Auto Format On Save özelliğini aktif etmemiz gerekiyor : 

```json
"editor.formatOnSave": true
```

## 4. TypeScript Temelleri

TypeScript, Javascript’e ek olarak “type” katmanını getiren, Javascript ‘deki zayıf tip kontrolünün aksine net bir tip tanımı ve dönüşümü sağlayan bir yapıdır ve .js yerine .ts dosyalarıyla ifade edilir. TypeScript ‘de arayüzler (interface) aracılığıyla tip belirlemeleri yapılır, veya örtülü tanımlanan değişkenler için değişken tipini TypeScript belirleyerek olası hatalar minimize edilir. Ayrıca tiplerle ilgili sorunlar compile işlemi sırasında (compile time) ortaya çıktığından; run-time esnasındaki hatalar minimum seviyeye indirgenir.

### 4.1. Type Inference

```tsx
let helloWorld = "Hello World";
```

Buradaki örtülü tanımda let ifadesi compile&time içerisinde şu ifadeye dönüştürülür : **`helloWorld : string`**

<aside>
💡 Bir örtülü değişkenin tipini öğrenmek için typeof kullanabiliriz : **`typeof helloWorld === “string”`** veya 
**`typeof helloWorld === “number”`** gibi.

</aside>

Şu şekilde de tanımlama yapabiliyoruz : 

```tsx
let helloWorld: string = "Hello World";
```

Ayrıca JSDoc ile yorum satırlarıyla metod dokümanları oluşturabiliriz.

```tsx
/**
 * @param {string} name
 * @return {string}
 */
function greet(name) {
    return `Hello, ${name}`; // ES6 : Template Literals
}
```

### 4.2. Interfaces

TypeScript ‘deki interfaceler C# ‘dakinden farklı olarak bir tip olarak kullanılabilir ve genelde kullanılacak olan nesneler ve bileşenler için “type” belirtmede kullanılırlar. C# da classlar ve structlar için kullanılan ve metod imzalarını tanımlayan arayüzlerden farklı olarak ; TypeScript ‘de daha alt kapsamlarda da kullanıma izin verilir.

```tsx
interface User {
	name : string;
	id : number;
}

const user : User {
	username : "Fatay",
	id : 0
}
```

Burada **`username`** propertysi interface içerisinde bulunmadığından ; compile time esnasında **`interface not contains “username” property`** şeklinde bir hata almış olacağız ; bu da TypeScript ‘in gücünü bize göstermektedir.

Bir diğer örnek (otomatik tip dönüşümü) : 

```tsx
interface User {
	name : string; 
	id : number;
}

class UserAccount {
	name : string; 
	id : number;
	
	constructor(name : string, id : number) {
		this.name = name;
		this.id = id;
	}
}

const user : User = new UserAccount ("Fatih", 1);
```

Interface ‘ler type olarak kullanılabildiğinden ; metod parametreleri veya dönüş tipleri için kullanılabilirler : 

```tsx
function deleteUser (user : User) { 
	// ... do something
}
```

```tsx
function getAdminUser () : User 
	// ... do something
}
```

---

### 4.3. TypeScript Türleri

TypeScript içerisinde tanımlı türler şunlardır : 

- boolean
- number
- string
- bigint
- null
- undefined
- tuple
- enum
- any
- unknown
- array
- object
- interface
- function
- generics

```tsx
// TypeScript ‘de string literals 
let lt = `string ${literal}`
// şeklinde kullanılabilir.
```

TypeScript için şu yapılar da önem taşır :

1. Generics
2. Struct
3. Composing Types
4. Mapped Types
5. Type Guards & Assertions
6. Conditional Types

Ek olarak TypeScript ‘de Any ve Unknown Türleri Bulunur : 

ANY (Dikkatli Kullanılmalıdır)

- TypeScript'e geçiş yapan büyük projelerde, geçici bir çözüm olarak kullanılır.
- Harici kitaplıklardan gelen ve tür bilgisi olmayan verilerle çalışırken kullanışlıdır.
- Tür güvenliğini kaybedersiniz. `any` kullanıldığında, TypeScript'in sunduğu hata yakalama özelliklerinden faydalanamayız.

```tsx
let value: any;
value = 42;
value = "Hello";
value = true;
```

UNKNOWN (With Type Safety)

- Unknown tipini bilmediğimiz (örneğin dış kaynaktan gelen bir veri için) kullanabileceğimiz type-safe bir yapıdır. ANY ‘den farklı olarak, typeof kullanmadan unknown tipindeki bir varlık için işlem yapamayız. “stringse böyle yap, number ise şöyle cast et vs.” gibi kullanımlara uygun olabilir.
- Bazen gerçekten kaynaktan gelen verilerin tipi uygulamanın anlık durumuna göre değişebilir. İhtiyaca göre dikkatli olmak şartıyla kullanılabilir.

```tsx
let value: unknown;
value = 42;
value = "Hello";
value = true;

// Tür kontrolü yapılmadan erişim sağlanamaz
if (typeof value === 'string') {
  console.log(value.toUpperCase());
}

```

Örnek TypeScript : Interface ve Class kullanımları 

```tsx
interface Person {
    firstName: string;
    lastName: string;
    age: number;
    getFullName(): string;
}
```

```tsx
class Student implements Person {
    firstName: string;
    lastName: string;
    age: number;
    studentId: number;

    constructor(firstName: string, lastName: string, age: number, studentId: number) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.age = age;
        this.studentId = studentId;
    }

    getFullName(): string {
        return `${this.firstName} ${this.lastName}`;
    }

    getStudentInfo(): string {
        return `ID: ${this.studentId}, Name: ${this.getFullName()}, Age: ${this.age}`;
    }
}
```

### 4.4. TypeScript ‘de Gelişmiş Türler

- Generics

```tsx
type StringArray = Array<string>;
type NumberArray = Array<number>;
type ObjectWithNameArray = Array<{ name: string }>
```

- Kendi Generic Sınıflarımızı Oluşturmak

```tsx
interface Backpack<Type> {
  add: (obj: Type) => void;
  get: () => Type;
}
 
// This line is a shortcut to tell TypeScript there is a
// constant called `backpack`, and to not worry about where it came from.
declare const backpack: Backpack<string>;
 
// object is a string, because we declared it above as the variable part of Backpack.
const object = backpack.get();
 
// Since the backpack variable is a string, you can't pass a number to the add function.
backpack.add(23);
Argument of type 'number' is not assignable to parameter of type 'string'.
```

- Struct Type (Değer Tipleri)

```tsx
interface Point {
  x: number;
  y: number;
}
 
function logPoint(p: Point) {
  console.log(`${p.x}, ${p.y}`);
}
 
// logs "12, 26"
const point = { x: 12, y: 26 };
logPoint(point);
```

- Composing Types (Birden Fazla Tip Belirtmek)

```tsx
// kendi bool türümüzü yazıyoruz :
type MyBool = true | false;

// veya her biri tip içerir : 
type WindowStates = "open" | "closed" | "minimized";
type LockStates = "locked" | "unlocked";
type PositiveOddNumbersUnderTen = 1 | 3 | 5 | 7 | 9;

// veya fonksiyonun birden fazla tipte değer dönmesini sağlayabiliriz : 
function getLength(obj: string | string[]) {
  return obj.length;
}
```

- Mapped Types (key-value türleri)

```tsx
type Readonly<T> = {
  readonly [P in keyof T]: T[P];
};
```

- Type Guards & Assertions

```tsx
function isString(value: unknown): value is string {
  return typeof value === "string";
}
```

- Conditional Types

```tsx
type NonNullable<T> = T extends null | undefined ? never : T;
```

### 4. 5. Decorators in TypeScript

Decorators, TypeScript'te sınıflara, metodlara, property'lere veya parametrelere ek işlevsellik katmak için kullanılan özel fonksiyonlardır. Bir decorator, bir sınıfın veya üyesinin davranışını değiştirmek veya genişletmek için kullanılan bir fonksiyondur. C# ‘daki [Attribute] yapılarına benzer ve metodlara farklı bir aspect kazandırmak için kullanılırlar.

**Decorator Türleri**

- **Class Decorators :** Bir sınıfın tanımını değiştirmek veya genişletmek için kullanılır.
- **Method Decorators :** Bir metodun davranışını değiştirmek veya genişletmek için kullanılır.
- **Accessor Decorators :** Bir property'nin getter veya setter metodlarını değiştirmek için kullanılır.
- **Property Decorators :** Bir property'nin davranışını değiştirmek için kullanılır.
- **Parameter Decorators :** Bir metodun parametresini değiştirmek için kullanılır.

Class Decorator Örneği : 

```tsx
function sealed(constructor: Function) {
    Object.seal(constructor);
    Object.seal(constructor.prototype);
    console.log(`Class ${constructor.name} is sealed.`);
}

@sealed
class Greeter {
    greeting: string;
    constructor(message: string) {
        this.greeting = message;
    }

    greet() {
        return `Hello, ${this.greeting}`;
    }
}

const greeter = new Greeter("world");
console.log(greeter.greet());
```

Method Decorator Örneği : 

```tsx
function log(target: Object, propertyKey: string, descriptor: PropertyDescriptor) {
    const originalMethod = descriptor.value;

    descriptor.value = function (...args: any[]) {
        console.log(`Calling ${propertyKey} with arguments: ${JSON.stringify(args)}`);
        const result = originalMethod.apply(this, args);
        console.log(`Result: ${result}`);
        return result;
    };

    return descriptor;
}

class Calculator {
    @log
    add(a: number, b: number): number {
        return a + b;
    }
}

const calculator = new Calculator();
console.log(calculator.add(2, 3));
```

## 5. Angular Komponent Yapısı

### 5.1. Angular ‘da Komponent Oluşturma

Angular ‘da yeni bir komponent oluşturmak için : 

```bash
ng generate component component-name
```

Sayfa ilk yüklenirken constructor çağrılarak bağımlılıklar yüklenir. Sonrasında ise Component LifeCycle içerisinde bulunan metodlar duruma göre tetiklenir. 

### 5.2. Angular ‘da Komponent Ömrü Metodları (Component Lifecycle Hooks)

1. **`ngOnInit() :`** 
    
    ***Çağrım Yöntemi :*** 
    
    - Bileşen hazır olduktan sonra ilk kez görüntülenmeye hazır hale geldiğinde çağrılır.
    
    ***Kullanımlar :*** 
    
    - Bir sayfa ilk açılırken gerekli olan verilerin servisten alınarak gösterilmesi gerektiğinde.
    - Bileşenle ilgili ilk ayarların veya değişkenlerin başlatılması gerektiğinde.

`*Örnek Kullanım : ngOnInit( ) ile Servisten Veri Çekme - Subscribe*`

```tsx
import { Component, OnInit } from '@angular/core';
import { DataService } from './data.service';

@Component({
  selector: 'app-example',
  templateUrl: './example.component.html',
})
export class ExampleComponent implements OnInit {

  data: any;

  constructor(private dataService: DataService) { }

  ngOnInit() {
    this.dataService.getData().subscribe(response => {
      this.data = response;
    });
  }
}
```

---

1. **`ngOnChanges() :`**
    
    ***Çağrım Yöntemi :*** 
    
    - Bileşenin `@Input` olarak aldığı veri her değiştiğinde çağrılır. Bu, bileşenin başlangıcında ve her veri değişikliğinde tetiklenir.
    
    ***Kullanımlar :*** 
    
    - Bileşene dışarıdan gelen verilerin değiştiği durumlarda bu değişikliklere tepki vermek gerektiğinde.
    - Parent bileşeninden gelen verilerdeki değişiklikleri işlemek ve bunlara göre bileşenin durumunu güncellemek için.
    
    `*Örnek Kullanım : ngOnChanges( ) ile Değişen @Input Değerini Okumak*` 
    
    ```tsx
    import { Component, Input, OnChanges, SimpleChanges } from '@angular/core';
    
    @Component({
      selector: 'app-child',
      templateUrl: './child.component.html',
    })
    export class ChildComponent implements OnChanges {
    
      @Input() inputData: string;
    
      ngOnChanges(changes: SimpleChanges) {
        if (changes.inputData) {
          console.log('inputData değişti:', changes.inputData.currentValue);
        }
      }
    }
    ```
    
    ---
    
2. **`ngDoCheck() :`**
    
    ***Çağrım Yöntemi :*** 
    
    - Angular’ın kendisinin tespit edemediği değişiklikleri tespit etmek için kullanılır. Bileşende bir değişiklik olup olmadığını manuel olarak kontrol eder.
    
    ***Kullanımlar :*** 
    
    - Angular'ın değişiklik tespit mekanizmasının (change detection) dışında kalan değişiklikleri kontrol etmek gerektiğinde.
    - Özellikle performans optimizasyonları veya karmaşık durumlarda manuel değişiklik tespiti yapmak için.
    
    `*Örnek Kullanım : ngDoCheck( ) ile External Change Detection*` 
    
    ```tsx
    import { Component, DoCheck } from '@angular/core';
    
    @Component({
      selector: 'app-example',
      templateUrl: './example.component.html',
    })
    export class ExampleComponent implements DoCheck {
    
      ngDoCheck() {
        console.log('ngDoCheck çalıştı');
      }
    }
    ```
    

---

1. **`ngAfterViewInit() :`**
    
    ***Çağrım Yöntemi :*** 
    
    - Bileşenin ve çocuk bileşenlerinin görünümleri (view) ilk kez oluşturulduğunda çağrılır.
    
    ***Kullanımlar :*** 
    
    - Bileşen görünümleri tamamen yüklendiğinde çalışması gereken kodlar için.
    - **`@ViewChild`** veya **`@ViewChildren`** ile alınan bileşen referanslarının erişilmesi gerektiğinde.
    
    `*Örnek Kullanım : ngAfterInit( ) ile Child Componentlerin Yönetilmesi*` 
    
    ```tsx
    import { Component, AfterViewInit, ViewChild } from '@angular/core';
    import { ChildComponent } from './child/child.component';
    
    @Component({
      selector: 'app-parent',
      templateUrl: './parent.component.html',
    })
    export class ParentComponent implements AfterViewInit {
    
      @ViewChild(ChildComponent) childComponent: ChildComponent;
    
      ngAfterViewInit() {
        console.log('Child component:', this.childComponent);
      }
    }
    ```
    
    ---
    
2. **`ngOnDestroy() :`** 
    
    ***Çağrım Yöntemi :*** 
    
    - Bileşen yok edilmeden hemen önce çağrılır.
    
    ***Kullanımlar :*** 
    
    - Temizlik işlemlerinin yapılması gerektiğinde (örneğin, event listener'ları kaldırmak veya unsubscribe işlemleri).

`*Örnek Kullanım : ngOnDestroy( ) ile Unsubscribe İşlemi*`

```tsx
import { Component, OnDestroy } from '@angular/core';
import { Subscription } from 'rxjs';
import { DataService } from './data.service';

@Component({
  selector: 'app-example',
  templateUrl: './example.component.html',
})
export class ExampleComponent implements OnDestroy {

  private dataSubscription: Subscription;

  constructor(private dataService: DataService) {
    this.dataSubscription = this.dataService.getData().subscribe();
  }

  ngOnDestroy() {
    this.dataSubscription.unsubscribe();
  }
}
```

## 6. Angular Servis Mimarisi

<aside>
💡 Angular ‘da typescript dosyasında servise erişim sağlayabiliriz fakat uygulamanın loosely-coupled (düşük bağımlılıkta) olmasını sağlamak için, servisleri uygulamadan ayırırız. Böylece SoC (Seperation of Concerns) prensibine de uyum sağlanır.

</aside>

Angular ‘da servis oluşturmak için :

```bash
ng generate service services/service-name
```

Genel ApiClient istemici servisimiz şu şekilde olabilir : 

```tsx
import { HttpClient } from '@angular/common/http';
import { Injectable } from '@angular/core';
import { Observable } from 'rxjs';
import { Options } from '../../types';

@Injectable({
  providedIn: 'root',
})
export class ApiService {
  constructor(private httpClient: HttpClient) {}

  get<T>(url: string, options: Options): Observable<T> {
    return this.httpClient.get<T>(url, options) as Observable<T>;
  }
}
```

Örneğin product servisi için ApiService ‘i  @inject ederek şu şekilde yazabiliriz : 

```tsx
import { Injectable } from '@angular/core';
import { ApiService } from './api.service';
import { Observable } from 'rxjs';
import { PaginationParams, Products } from '../../types';

@Injectable({
  providedIn: 'root',
})
export class ProductsService {
  constructor(private apiService: ApiService) {}

  getProducts = (
    url: string,
    params: PaginationParams
  ): Observable<Products> => {
    return this.apiService.get(url, {
      params,
      responseType: 'json',
    });
  };
}
```

Burada product servisi constructor ‘ı içerisinde apiService inject ettik ve subscribe edeceğimiz Observable nesnesinin tipini Products olarak belirledik.

# Angular 18 : Decorators

## 7. Decorator Pattern & Decorators in Angular

***Decorator ler***, decorator design pattern uygulayarak  C# ‘daki [Attribute] ve Reflection yapısıyla kullanımda veya Jetpack Compose ‘daki yine composable component (@Composable) yapısına benzer bir şekilde belirtilen fonksiyonun veya sınıfın çalışma esnasındaki hareketini değiştirmek için kullanılır.

### 7.1. Decorator Pattern Fundamentals

<aside>
💡 *In object-oriented programming, the decorator pattern is a design pattern that allows behavior to be added to an individual object, dynamically, without affecting the behavior of other instances of the same class.[1] The decorator pattern is often useful for adhering to the Single Responsibility Principle, as it allows functionality to be divided between classes with unique areas of concern[2] as well as to the Open-Closed Principle, by allowing the functionality of a class to be extended without being modified.[3] Decorator use can be more efficient than subclassing, because an object's behavior can be augmented without defining an entirely new object.*

</aside>

**What problems can it solve?**

- Responsibilities should be added to (and removed from) an object dynamically at run-time.
- A flexible alternative to subclassing for extending functionality should be provided.

**Further Reading**

[https://en.wikipedia.org/wiki/Decorator_pattern](https://en.wikipedia.org/wiki/Decorator_pattern)

[https://www.typescriptlang.org/docs/handbook/decorators.html](https://www.typescriptlang.org/docs/handbook/decorators.html)

[https://refactoring.guru/design-patterns/decorator/csharp/example](https://refactoring.guru/design-patterns/decorator/csharp/example)

[https://blog.logrocket.com/simplifying-codebase-swift-decorator-design-pattern/](https://blog.logrocket.com/simplifying-codebase-swift-decorator-design-pattern/)

[https://www.blog.finotes.com/post/decorator-pattern-in-jetpack-compose-android-apps](https://www.blog.finotes.com/post/decorator-pattern-in-jetpack-compose-android-apps)

---

## 7. 2. Angular 'da @ViewChild, @Input, @Output Dekorasyonları

**`@Input Dekorasyonu`**

- `@Input` , bir bileşenin (child component) dışarıdan veri almasını sağlar. Bu sayede, parent component, child component'a veri gönderebilir. React ‘daki propslar gibi çalışıyor.

Örneğin parent komponentimiz şöyle olsun : 

```html
<app-child [data]="parentData"></app-child>
```

Child komponent datayı şu şekilde @Input olarak almalı : 

```tsx
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-child',
  template: `<p>{{ data }}</p>`
})
export class ChildComponent {
  @Input() data: string;
}
```

---

**`@Output Dekorasyonu`**

- `@Output` dekoratörü ve `EventEmitter` sınıfı, child komponentimizden parente veri göndermeyi sağlar. Genellikle event tabanlı işlemler için kullanılır.

Örneğin parent komponentimiz şöyle olsun : 

```tsx
<app-child (childEvent)="handleChildEvent($event)"></app-child>
```

Child component üzerinden parent ile event tetikleyebiliriz :

```tsx
import { Component, Output, EventEmitter } from '@angular/core';

@Component({
  selector: 'app-child',
  template: `<button (click)="sendEvent()">Click me</button>`
})
export class ChildComponent {
  @Output() childEvent = new EventEmitter<string>();

  sendEvent() {
    this.childEvent.emit('Hello from child!');
  }
}
```

---

**`@Output Dekorasyonu`**

- `@ViewChild` dekoratörü, bir bileşen veya direktifinin referansını almak için kullanılır. Bu sayede, bileşen içinde başka bir bileşene veya template öğesine erişim sağlanabilir.

Komponent şablonumuz şöyle olsun : 

```tsx
<app-child #childComponent></app-child>
<button (click)="accessChild()">Access Child</button>
```

Parent komponente child komponente erişim :

```tsx
import { Component, ViewChild, AfterViewInit } from '@angular/core';
import { ChildComponent } from './child/child.component';

@Component({
  selector: 'app-parent',
  templateUrl: './parent.component.html'
})
export class ParentComponent implements AfterViewInit {
  @ViewChild('childComponent') child: ChildComponent;

  ngAfterViewInit() {
    console.log(this.child);
  }

  accessChild() {
    console.log(this.child.data);
  }
}
```

<aside>
💡 ViewChild ve diğer dekoratörlerin ve lifecycle metodlarının daha detaylı olarak kullanımlarına bakılacak.

</aside>

Fatih Aydin
