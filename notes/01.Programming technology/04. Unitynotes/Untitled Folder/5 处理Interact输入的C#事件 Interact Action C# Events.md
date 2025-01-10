## 5 处理Interact输入的C#事件 Interact Action C# Events

### 5.1 添加Interact Action

打开PlayerInputActions.inputactions，添加一个Action，命名为Interact，绑定E键

![img](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/2023_2/pic/202311150428002.png)

在GameInput.cs中，我们使用委托为这个Interact Action添加一个调用的函数Interact_performed()，我们先来测试一下，使用Debug.log(obj)在控制台输出一下调用的函数本身



```C#
 // GameInput.cs中
 using UnityEngine;
 
 public class GameInput : MonoBehaviour
 {
     private PlayerInputActions playerInputActions;
     
     private void Awake()
     {
         playerInputActions = new PlayerInputActions();
         playerInputActions.Player.Enable();
         
         playerInputActions.Player.Interact.performed += Interact_performed;
     }
     
     private void Interact_performed(UnityEngine.InputSystem.InputAction.CallbackConC# obj)
     {
         Debug.Log(obj);
     }
 
     public Vector2 GetMovementVectorNormalized()
     {
         Vector2 inputVector = playerInputActions.Player.Move.ReadValue<Vector2>();
         
         inputVector = inputVector.normalized;
         return inputVector;
     }
 }
```



> [Unity Input System文档](https://link.zhihu.com/?target=https%3A//docs.unity3d.com/Packages/com.unity.inputsystem@1.0/manual/Actions.html)

![img](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/2023_2/pic/202311150428003.png)





启动游戏，按下E键，可以看到控制台输出了我们的按下对应按键的相关信息

![img](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/2023_2/pic/202311150428004.png)





### 5.2 使用EventHandler委托将交互逻辑写在Player.cs

接下来在Interact Action调用的函数Interact_performed()中添加EventHandler委托，在Player.cs中为该委托添加具体的交互行为



```C#
 // GameInput.cs中
 using System;
 using UnityEngine;
 
 public class GameInput : MonoBehaviour
 {
     public event EventHandler OnInteractAction; // 新添加
     
     private PlayerInputActions playerInputActions;
     
     private void Awake()
     {
         playerInputActions = new PlayerInputActions();
         playerInputActions.Player.Enable();
         
         playerInputActions.Player.Interact.performed += Interact_performed;
     }
     
     private void Interact_performed(UnityEngine.InputSystem.InputAction.CallbackConC# obj)
     {
         OnInteractAction?.Invoke(this, EventArgs.Empty); // 新添加
         // 这里使用了"?"运算符，与下面的代码相同
         // if (OnInteractAction != null)
         // {
         //     OnInteractAction(this, EventArgs.Empty);
         // }
     }
 
     public Vector2 GetMovementVectorNormalized()
     {
         Vector2 inputVector = playerInputActions.Player.Move.ReadValue<Vector2>();
         
         inputVector = inputVector.normalized;
         return inputVector;
     }
 }
```



这里可以在GameInput_OnInteractAction()中先放上之前HandleInteractions()的代码测试一下



```C#
 // Player.cs中 
 ...
  private void Start()
  {
       gameInput.OnInteractAction += GameInput_OnInteractAction;
  }
  
  private void GameInput_OnInteractAction(object sender, EventArgs e)
  {
       //将原先HandleInteractions()中的内容暂时放到了这里，并且暂时不再调用HandleInteractions()中的clearCounter.Interact()
       Vector2 inputVector = gameInput.GetMovementVectorNormalized();
       
       Vector3 moveDir = new Vector3(inputVector.x, 0f, inputVector.y);
       
       if (moveDir != Vector3.zero)
       {
             lastInteractDir = moveDir;
       }
       
       float interactDistance = 2f;
       if (Physics.Raycast(transform.position, lastInteractDir, out RaycastHit raycastHit,interactDistance, countersLayerMask))
       {
             if (raycastHit.transform.TryGetComponent(out ClearCounter clearCounter))
             {
                  // Has ClearCounter.cs
                  clearCounter.Interact();
             }
       }
  }
  ...
```



这时运行游戏按E进行交互，可以得到显示Interact（我们先前在clearCounter.Interact()中定义了输出"Interact"）

![img](https://cnchu-1310638968.cos.ap-nanjing.myqcloud.com/2023_2/pic/202311150428005.png)





> 个人解释：使用`playerInputActions.Player.Interact.performed += Interact_performed;`是为该控制器触发的委托添加了一个会调用的函数，在这个函数中，我们写具体的按下按键发生的事情，这里我们想把具体的交互行为写在Player.cs中，所以我们在这个函数中去触发一个叫`OnInteractAction`的Eventhandler委托，这个委托添加的会调用的函数写在了Player.cs中，使用`gameInput.OnInteractAction += GameInput_OnInteractAction;`添加了`GameInput_OnInteractAction()`这个具体处理输入逻辑函数 关于委托和事件的讲解可以看[刘铁猛老师的C#课程](https://link.zhihu.com/?target=https%3A//www.bilibili.com/video/BV13b411b7Ht%3Fp%3D19%26vd_source%3Dc01a7b5440706d76efa61acaf26acff7)和[CodeMonkey讲C#的相关课程](https://link.zhihu.com/?target=https%3A//www.youtube.com/playlist%3Flist%3DPLzDRvYVwl53t2GGC4rV_AmH7vSvSqjVmz)



目前为止的[工程文件](https://link.zhihu.com/?target=https%3A//unitycodemonkey.com/kitchenchaoscourse.php%23interactActionCSEvents)