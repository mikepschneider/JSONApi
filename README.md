# JSONApi
A simple way to implement JSONApi specifications to convert Models to Json and Json to Models.

## INSTALL
Add this dependecy from jCenter:
*Awaiting dependecy*

## USAGE
The first step to use the library is to initiate the deserializer with your classes.
To show how it works, we will use the default JSON that is on jsonapi.org homepage and on [raw folder](/app/src/main/res/raw/data.json).

### FIRST STEP - *Create your models*
All models to be conveted need to have two things.
* It need to inherit from [Resource](/JsonAPI/src/main/java/com/gustavofao/jsonapi/Models/Resource.java)
* It need to have the [Type](/JsonAPI/src/main/java/com/gustavofao/jsonapi/Annotations/Type.java) annotation on your class.

```java
import com.gustavofao.jsonapi.Annotatios.Type;
import com.gustavofao.jsonapi.Models.Resource;

@Type("comments")
public class Comment extends Resource {

    private String body;
    private Person author;

    public String getBody() {
        return body;
    }

    public void setBody(String body) {
        this.body = body;
    }

    public Person getPerson() {
        return author;
    }

    public void setPerson(Person author) {
        this.author = author;
    }
}
```

### SECOND STEP - *Instantiate the JSONApiConverter*
The JSONApiConverter have to be instantiated with all your models.

```java
JSONApiConverter api = new JSONApiConverter(Article.class, Comment.class, Person.class);
```

### THIRD STEP - *Serialize or deserialize*

#### SERIALIZE INTO JSON
To serialize one object, it have to be an instance or inherit from Resource and have to be passed as parameter to *toJson*.
The return will be a String with the JSON.

```java
Article article = new Article();

//
//       SET VALUES HERE
//

String jsonValue = api.toJson(article);
```

> **WARNING**
> LINKS FIELDS ARE NOT GOING TO BE SERIALIZED. I'M WORKING ON IT FOR THE NEXT VERSION.

#### DESERIALIZE FROM JSON
To deserialize the JSON, you have to pass it as parameter for *fromJson* method.
The return will be an JSONApiObject.

```java
JSONApiObject obj = api.fromJson(json);
if (obj.getData().size() > 0) {
    //Success   
    if (obj.getData().size() == 1) {
        //Single Object  
        Article article = (Article) obj.getData(0);
    } else {
        //List of Objects
        List<Resource> resources = obj.getData();
    }
} else {
    //Error or empty data
}
```

> **WARNING**
> DATA WILL ALWAYS COME AS LIST. YOU HAVE TO VERIFY IF THERE IS ONLY ONE OR MORE. I'M WORKING ON IT TOO.

### TIPS
#### ONE-TO-MANY RELATION
To handle with one-to-many relation, you have to use [JSONList](/JsonAPI/src/main/java/com/gustavofao/jsonapi/Models/JSONList.java) with the type of the Object.
Example below.

#### CHANGE SERIALIZATION NAME
To change the name of the object on the JSON, you can use the Annotation [/JsonAPI/src/main/java/com/gustavofao/jsonapi/Annotations/SerialName.java](SerialName) on your field.
Example below.

#### IGNORE FIELDS
To ignore fields of the model, you have to use the Annotation [Excluded](/JsonAPI/src/main/java/com/gustavofao/jsonapi/Annotations/Excluded.java) on your field.
Example below.

```java
import com.gustavofao.jsonapi.Annotatios.Excluded;
import com.gustavofao.jsonapi.Annotatios.SerialName;
import com.gustavofao.jsonapi.Annotatios.Type;
import com.gustavofao.jsonapi.Models.JSONList;
import com.gustavofao.jsonapi.Models.Resource;

@Type("articles")
public class Article extends Resource {

    private String title;
    private JSONList<Comment> comments;
    
    @Excluded 
    private String blah;

    @SerialName("author")
    private Person person;

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public Person getPerson() {
        return person;
    }

    public void setPerson(Person person) {
        this.person = person;
    }

    public JSONList<Comment> getComments() {
        return comments;
    }

    public void setComments(JSONList<Comment> comments) {
        this.comments = comments;
    }
}

```

## Thanks
[AceleraMEI](http://www.aceleramei.com.br/) for suport the development.

## License
    Copyright 2016 Gustavo Fão. All rights reserved.

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

        http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.