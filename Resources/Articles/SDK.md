* [SDK](#sdk)
  * [Application States](#application-states)  

# SDK

## Application States
The iOS application states are as follows:

### 1) Not running state: 
The app **has not been launched** or was running but was **terminated by the system**.  

#### 2) Inactive state:
The app is running in the **foreground** but is currently **not receiving events**. 

(It may be executing other code though.) 
* 通常只會短暫地處於 inactive state
  >An app usually stays in this state only **briefly** as it **transitions to a different state**. 
* 除非使用者 lock the screen 或是系統讓使用者回應一些其他的 events，才會在此狀態停留較久
  >The only time it stays inactive for any period of time is when the user locks the screen or the system prompts the user to respond to some event (such as an `incoming phone call` or `SMS message`).  

#### 3) Active state: 
The app is running in the **foreground** and is **receiving events**. 
This is the normal mode for foreground apps.  

#### 4) Background state:
The app is in the **background** and **executing code**. 

* 通常也是短暫地處於此狀態，在 app 被 suspended 之前
  > Most apps enter this state **briefly** on their way to being suspended. 
* 除非有特定的 request
  > However, an app that **requests extra execution time** may remain in this state for a period of time. 
* In addition, an app being **launched directly into the background** enters this state instead of the inactive state.  

#### 5) Suspended state:
While suspended, an app **remains in memory** but does **not execute any code**.
When a **low-memory condition** occurs, the system may purge suspended apps without notice to make more space for the foreground app.