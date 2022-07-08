# Factory Pattern
It allows to us to create instances of different concrete objects of the same family (implements the same instance).
![updated_abstract_factory](https://user-images.githubusercontent.com/5640913/177965066-96f0b7b1-0f61-49cb-b43c-af7453499389.jpg)
## Example
We can use a social network factory as example to get elements of different social networks
```
instance SocialNetwork {
  public getElements: Array<Element>;
}

class Facebook implements SocialNetwork {
  public Array<Element> getElements()
  {
    // do stuff
  }
}

class Instagram implements SocialNetwork {
  public Array<Element> getElements()
  {
    // do stuff
  }
}

class Twitter implements SocialNetwork {
  public Array<Element> getElements()
  {
    // do stuff
  }
}

class SocialNewtworkFactory {
  public SocialNetwork getSocialNetwork(string socialNewtworkName)
  {
    switch (socialNewtworkName){
      case 'Facebook':
        return new Facebook();
       case 'Twitter':
        return new Twitter();
       case 'Instagram':
        return new Instagram();
    }
  }
}
