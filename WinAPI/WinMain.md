기본적인 WinAPI의 구조 및 라이프 사이클
======================================

 C++을 사용한 자체 엔진 개발을 위해 기본적으로 VS의 **Windows 데스크톱 마법사**를 사용하여 시작을 할 것이다.
 위부터 차례로 함수 하나씩 짚어갈 것이다. 

# 1. wWinMain 함수
wWinMain 함수를 부분적으로 알아보겠다.
 ## Parameter (매개변수)
```
int APIENTRY wWinMain(_In_ HINSTANCE hInstance,
                     _In_opt_ HINSTANCE hPrevInstance,
                     _In_ LPWSTR    lpCmdLine,
                     _In_ int       nCmdShow)
```
wWinMain 함수의 파라미터는 4가지가 존재한다.

HINSTANCE hInstance는 프로그램의 인스턴스 핸들이다. 
  ### 핸들이란?
   특정 리소스나 메모리와 같이 직접적으로 접근할 수 없는 곳에 간접적으로 접근할 수 있도록 시스템이나 라이브러리가 제공하는 추상화된 참조이다. 포인터와 비슷하나 핸들은 실제 메모리 주소를 직접적으로 노출하지 않는다. 각 리소스의 고유 식별자 역할로도 쓰인다.
   
HINSTANCE hPrevInstance는 직전에 실행된 프로그램의 인스턴스 핸들이다. (현재는 잘 사용하지 않는다함. 현재는 NULL 전달.)
 
LPWSTR lpCmdLine은 명령행 인자로 프로그램 시작시 전달되는 정보이다. 

int nCmdShow는 윈도우가 표시되는 방법을 제어한다.

파라미터는 위 4가지로 구성되어있다. 해당 프로그램의 핸들, 직전 프로그램의 핸들, 프로그램 시작시 전달할 정보, 윈도우 표시 방법을 전달 받는다.

## 문자열 초기화
```
 LoadStringW(hInstance, IDS_APP_TITLE, szTitle, MAX_LOADSTRING);
 LoadStringW(hInstance, IDC_EDITORWINDOW, szWindowClass, MAX_LOADSTRING);
```
앱의 타이틀과 에디터 창의 이름을 초기화 하는 곳이다. 

## MyRegisterClass -창 클래스 초기화 및 등록
```
ATOM MyRegisterClass(HINSTANCE hInstance)
{
    WNDCLASSEXW wcex;

    wcex.cbSize = sizeof(WNDCLASSEX);

    wcex.style          = CS_HREDRAW | CS_VREDRAW;
    wcex.lpfnWndProc    = WndProc;
    wcex.cbClsExtra     = 0;
    wcex.cbWndExtra     = 0;
    wcex.hInstance      = hInstance;
    wcex.hIcon          = LoadIcon(hInstance, MAKEINTRESOURCE(IDI_EDITORWINDOW));
    wcex.hCursor        = LoadCursor(nullptr, IDC_ARROW);
    wcex.hbrBackground  = (HBRUSH)(COLOR_WINDOW+1);
    wcex.lpszMenuName   = MAKEINTRESOURCEW(IDC_EDITORWINDOW);
    wcex.lpszClassName  = szWindowClass;
    wcex.hIconSm        = LoadIcon(wcex.hInstance, MAKEINTRESOURCE(IDI_SMALL));

    return RegisterClassExW(&wcex);
}
```
 빌드 시 뜨게되는 창의 기본 설정을 하는 곳이다.
