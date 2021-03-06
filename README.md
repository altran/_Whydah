Whydah
======

Whydah is an Identity and Single Sign-On solution that provides advanced role-based access control and flexible integration's.  This is the top-level repository for the Whydah components.


### Quick set-up (Using Docker on local machine)

* Install docker
* Start Whydah
```
sudo docker run -it -p 80:9999 -p 9990:9990 -p 9995:9995 -p 9996:9996 -p 9997:9997 -p 9998:9998  totto/whydah /usr/bin/supervisord 
```
* Go to Whydah [http://localhost/sso/welcome]  (admin/whydahadmin)

### Client code example

*Example using Apache HTTP Components Fluent API and jOOX Fluent API*
```
//  Execute a POST to authenticate my application
String appToken = Request.Post("https://sso.whydah.net/sso/logon")
        .bodyForm(Form.form().add("applicationcredential", myAppCredential).build())
        .execute().returnContent().asBytes();

//  authenticate with username and password (user credential)
String usertoken = Request.Post("https://sso.whydah.net/sso/user/"+appTokenID+"/"+new UserTicket(UUID.randomUUID()).toString()+"/usertoken/")
        .bodyForm(Form.form().add("apptoken", appToken)
        .add("usercredential", new UserCredential(username,password).asXML()).build())
        .execute().returnContent().asBytes();

//  Execute a POST  to SecurityTokenService with userticket to get usertoken
String usertoken = Request.Post("https://sso.whydah.net/sso/user/"+appTokenID+"/get_usertoken_by_userticket/")
        .bodyForm(Form.form().add("apptoken", appToken)
        .add("userticket", userTicket).build())
        .execute().returnContent().asBytes();

// That's all you need to get a full user database, IAM/SSO, Facebook/OAUTH support ++
boolean hasEmployeeRoleInMyApp = $(usertoken).xpath("/usertoken/application[@ID="+myAppId+"]/role[@name=\"Employee\"");
```
![Sequence Diagram](https://raw.githubusercontent.com/altran/Whydah/master/images/Integration%20-%20simple%20standalone.png)



![Architectural Overview](https://raw.githubusercontent.com/altran/Whydah/master/images/Whydah%20infrastructure.png)



### Infrastructure setup components

We plan to build a software-defined network application to control and handle various configuration of Whydah production setups. As they are developed they will arrive and be listed and documented here.



#### Whydah node configurations

To make it easy to adopt and evolve Whydah components, we'll make ready-to use Docker containers of all the Whydah modules, both as Docker images and the corresponding Dockerfile-configurations to make it easy to just grab a complete component or adjust and build your own.

#### Docker: UIB configurations

* OpenLdap ansible configuration  [https://github.com/javaBin/Whydah-Provisioning/tree/master/roles/openldap]
* OpenLdap dockerfile   [https://github.com/altran/Whydah/tree/master/config/Docker/uib/uib-ldap]
* UIB all-in-one image  [https://registry.hub.docker.com/u/totto/whydah-uib-all-in-one/]
* UIB all-in-one dockerfile  [https://raw.githubusercontent.com/altran/Whydah/master/config/Docker/uib/uib-all-in-one/Dockerfile]

####  Docker: UAS configurations

* UAS Docker image [https://registry.hub.docker.com/u/totto/whydah-uas]
* UAS Dockerfile [https://raw.githubusercontent.com/altran/Whydah/360f0d821d95eecb3cfd6ae3628a622b5a0c0a63/config/Docker/uas/Dockerfile]

####  Docker: STS configurations

* STS Docker image [https://registry.hub.docker.com/u/totto/whydah-sts]
* STS Dockerfile [https://raw.githubusercontent.com/altran/Whydah/27bf84cad672af4985e9142a129e5880d02d2984/config/Docker/sts/Dockerfile]

#### Docker: SSOLWA configurations

* SSOLWA Docker image [https://registry.hub.docker.com/u/totto/whydah-ssolwa}
* SSOLWA Dockerfile [https://raw.githubusercontent.com/altran/Whydah/930dc65bdbed653b717603e09f590603c8d57283/config/Docker/ssolwa/Dockerfile]

####  Docker: UAWA configurations

* UAWA Docker image [https://registry.hub.docker.com/u/totto/whydah-uawa]
* UAWA Dockerfile [https://raw.githubusercontent.com/altran/Whydah/360f0d821d95eecb3cfd6ae3628a622b5a0c0a63/config/Docker/uawa/Dockerfile]


### Ansible:  Ansible Whydah provisioning

For those who prefer using Ansible to provision solutions, we suggest that you fork our general 
ansible provisioning repository on github and adjust it according to youur needs

* [https://github.com/altran/Whydah-Provisioning]



### Documentation:

* Overview (http://getwhydah.com/)
* Installation, Architecture and Development (https://wiki.cantara.no/display/whydah/Whydah+Home)
* Source Code (https://github.com/search?o=desc&q=Whydah&s=updated&type=Repositories&utf8=%E2%9C%93)



