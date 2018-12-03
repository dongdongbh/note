---
category: Stuff
path: '/stuff/:id'
title: 'remote visit https(jb)'
type: 'TIPS'

layout: nil
---

## jupyter and tensorboard configuration

#### Visit jupyter notebook without token and open as public server

1. on server run `jupyter notebook --generate-config`

2. open `ipython` and run

   ```
   In [1]: from notebook.auth import passwd
   In [2]: passwd()
   Enter password: 
   Verify password: 
   Out[2]: 'sha1:ce23d945972f:34769685a7ccd3d08c84a18c63968a41f1140274'In [1]: from 
   ```

   Here, the password is for you to log in jupyer notebook to replace the random token on browser

3. ```
   vi ~/.jupyter/jupyter_notebook_config.py
   ```

   add following text to it

   ```
   # Set ip to '0.0.0.0' to bind on all interfaces (ips) for the public server
   c.NotebookApp.ip = '0.0.0.0'
   c.NotebookApp.password = u'sha1:0xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx' #the output of last step
   c.NotebookApp.open_browser = False
   c.NotebookApp.port =8889 #指定一个端口
   ```

4. visit jupyter by https://localhost:8889 or https://serverip:8889 and input the password you set before and enjoy it!

#### tensorboard open as public server

run on server

```
tensorboard --logdir=runs --host=0.0.0.0 --port=6006
```

### Jupyter notebook and tensorboard on remote

----

### if you configured it as a public server

Set ip to `c.NotebookApp.ip = '0.0.0.0'` to bind on all interfaces (ips) for the **public server**

* #### M1

 on your computer add following to './ssh/config'

> Host jupyter
> ​	HostName 172.16.101.136
> ​	LocalForward 8889 172.16.0.248:8889
> ​	User lidd
>
> Host tensorboard
> ​	HostName 172.16.101.136
> ​	LocalForward 8008 172.16.0.248:6006
> ​	User lidd

then run 

```
ssh -f -N jupyter
ssh -f -N tensorboard
```

* #### M2

 or just run

```bash
ssh -N -f -L 8889:172.16.0.248:8889 lidd@172.16.101.136
```

### if you didn't configure it as a public server

> note: in this case, jupyter notebook only can be access by [localhost], so we must forward ssh tunnel twice

#### Method 1：

1. on jumper :

   add following to './ssh/config'

   ```
   Host jupyter_dd
   	HostName 172.16.0.248
   	LocalForward 10048 localhost:8889
   	User lidd
   Host tensorboard_dd 
   	HostName 172.16.0.248
   	LocalForward 10049 127.0.0.1:6006
   	User lidd
   ```

   and then run 

   ```
   ssh -f -N jupyter_dd
   ssh -f -N tensorboard_dd
   ```

   Here, I am not sure why tensorboard need '127.0.0.1' but not 'localhost'

2. on your computer add following to './ssh/config'

   ```
   Host jupyter
   	HostName 172.16.101.136
   	LocalForward 8889 localhost:10048
   	User lidd
   Host tensorboard
   	HostName 172.16.101.136
   	LocalForward 8008 localhost:10049
   	User lidd
   ```

   and then run 

   ```bash
   ssh -f -N jupyter
   ssh -f -N tensorboard
   ```

   ​	

#### Method ２：

1. on jumper :

   ```bash
   ssh -f -N -L 10048:localhost:8889 lidd@172.16.0.248
   ```

2. on your computer 

   ```bash
   ssh -f -N -L 8889:localhost:10048 lidd@172.16.101.136
   ```

   then run jupyter notebook and tensorboard on server by running

   ```bash
   jupyter notebook --no-browser --port=8889 upyter notebook --no-browser --port=8889
   tensorboard --logdir=runs --host localhost 
   ```

   then you can visit jupyter by https://localhost:8889 and https://locahost:8008 , but you have to input token on jupyter

   > notice: if you use relative dirctionary, you must **run tensorboard in the dirctionary**, otherwise tensorboard con't find your log data to show



ref. [SSHMenu Articles](http://sshmenu.sourceforge.net/articles/)


