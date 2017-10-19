##**Google Play l App Excellence Program 9월**

> Android O - New keyword

1. Alert Window - display over the window

2. Foreground Service Notification - Don’t set FG notification Priority to MIN - gym day 발생했던 문제 ( Nexus 6p )

3. - Importance Low is recommended 
   - 해당 포그라운드 서비스를 돌리고 있을때 거기에 붙어있는 노티피케이션을 중요도를 낮춰라. 그러면 안드로이드 시스템 노티피케이션이 뜨는것을 막을 수 있음.

4. Short Cut - 앱 바로가기 아이콘 

5. - Using ShortcutManager.requserPinShortcut —>
   - Deprecating INSTALL_SHORTCUT broadcast

6. Webview safe browsing

7. - android.webkit.webview.EnableSafeBrowsing

8. Android ID - 디바이스 유니크 식별자

9. - Will be changed when a user uninsatalled and then reinstalled after the OTA.
   - 하위호환성을 위해서 안드로이드 아이디값을 보장하기 위해 7.0에서 사용하다가 8.0 올리면 그래도 그대로 유지 but,  앱을 지우고 다시설치하면 변경될 수는 있다.

10. Support LIb min SDK 14 ~ 26.0

> # Android O what’s in New

1. Notifications

2. - Notification Channels - set up category. / Priority 적용하기

3. 1. - Top : Major ongoing - media session , navigation, foreground setcvice notifications may use setColozed()

      - Middle : people to people/ MessagingStyle notification then General

      - Bottom : min notification ( by the way )

        ​

4. Notification Badges or Dots - 님이 안읽은 노티피케이션이 있음을 알려주고 기본적으로 닷이 뜨고 롱프레스 일떄 숏컷이 나옴 / 점을 찍거나 안찍도록 노티피케이션을 설정할 수있고, 색상은 바꿀 수 없고 , 바꾸려면 아이콘의 색상을 바꺼야함 (임의로 바뀜)

5. Pinning Shortcut & widgets

6. Picture in Picture

7. Downloaderable Text Font

8. Chached Data -  StorageManager ( 캐시 사이즈 조절 , GC 막??)

9. Broadcast removal - boot complete !! 확인 ㄱ ㄱ 

10. !!! Explicit broadcast are still allow

11. Background Service Limits - 백그라운드 서비스 1분정도 있다가 onStop()이 불리는 효과가 나옴.

12. 그때부터 백그라운드로 내려가면 서비스를 호출하여 살리고 싶어도 1분뒤에 서비스가 illegalstateException을 내뿜는다

13. CTS test will be in place for android defined intents 

14. Job intent service - compliant하게 O 이상에서는 job scheduler 에 신경안쓰고 쓸 수 있다.

15. Sync Adapter , arlam manager , Alarms, Jobs and Services 

16. Location is powerful - 배터리 소모량 많음