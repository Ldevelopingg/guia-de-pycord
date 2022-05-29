<div align="center">
  <h1> ¡Hola!, bienvenid@ a la guía NO OFICIAL de pycord </h1>
  
  **```Introducción: LA CREACIÓN DEL BOT```**
  
  **```La creación del bot es en realidad bastante sencilla, simplemente tienes que dirigirte a la página de:```** **[Discord Developer Portal](https://discord.com/developers/docs/intro)** 
 
  **```Y seguir los siguientes pasos 👇```**
  
  >**```Paso 1: Ir al apartado de "Aplications", ahí se te pedirá el acceso a tu cuenta por lo cual tendrás que iniciar sesión```**
  
  
  >**```Paso 2: Ir al apartado de "New Aplication" que se encuentra en la esquina superior derecha y tendrás que ingresar el nombre del bot,  si ya lo ingresaste ¡Muy bien!, acabas de crear tu primer aplicación (si es que no tenías una antes jaja), completados estos pasos vámonos al penúltimo paso```**
  
  >**```¡Ya estás muy cerca de crear el bot! tienes que dirigirte al apartado de "Bot" y presionar el botón que dice: add bot, después te aparecerá una ventana que te dirá que la acción de crear un bot es irreversible y bla bla, solamente aprieta el botón de "Yes, do it" y ¡listo! ya tienes creado el bot```**
  
 > **```Como último paso y el más importante: tienes que dar click en el botón que dice "reset token" y después copiarlo, más adelante veremos para qué funcions el token```**
  
 **Espero que todo te haya salido bien hasta este momento, si no, déjame un comentario y yo trataré de responder**
  
  <div align="center">
    <h1> Codificar el bot </h1>
    
**```Bienvenido a ésta nueva sección, aquí encontrarás el código básico para poder encender tu bot y mostrar su latencia, ¿Estás list@? ¡Vamos!```**

> **```Paso 1: Tienes que dirigirte al editor de código de tu elección, también puedes crear un repositorio en una página web como replit o github```**
    
    
> **```Paso dos: Ya que tu repositorio esté listo tendrás que crear un archivo .py, puedes asignarle el nombre que quieras, pero de preferencia un "main.py"```¨**
    
    
> **```Paso 3: Ahora tienes que hacer las instalaciones y importar los módulos, abajo te dejaré el código```**
    
<div align="center">
  <h1> Instalación </h1>
    
```py
#instalar pycord  
pip install pycord
#instalar pycord 2.0
py -3 -m pip install -U git+https://github.com/Pycord-Development/pycord
#instalarlo en macOS
python3 -m pip install -U pycord
#instalarlo en windows
py -3 -m pip install -U pycord
#instalarlo en linux
sudo apt-get install pycord
```

<div align="center">
  <h1> Módulos </h1>

```py
import discord
import asyncio
```
  
 **```Paso 4: Ahora tendrás que insertar este código para definir el prefijo, hacer tu primer comando y encender al bot```**
  
  <div align="center">
    <h1> Código básico </h1>
  
```py
client = commands.Bot(command_prefix = 'el prefijo que desees', help_command = None) #definiendo el prefijo
    
@client.event
async def on_ready():
  print(f"el bot está listo")    #cuando el bot esté prendido se mostrará en consola
    
```
  
```py  
@client.command()
async def ping(ctx):
    await ctx.send(f"> Pong! :ping_pong: **{round(client.latency * 1000)}**ms") #comando para mostrar la latencia
```
 
```py
client.run('el token del bot') #encender al bot. Nota: mantener el token secreto es muy importante ya que es la llave para poder configuar al bot así que no se lo compartas a NADIE a menos que sea de mucha confianza, si el token se llega a filtrar resetealo, solo tendrás que ingresar el nuevo token en client.run('token')
```  
  
**```Si todo salió bien hasta aquí: ¡Felicidades!, tu bot ya tiene su primer comando y está encendido, si no, verifica que hayas seguido todos los pasos correctamente```**
    
<div align="center">
  <h1> Codificando al bot pt2 </h2>
  
 **```Si llegaste hasta aquí es porque ya tienes listo el código básico, ahora pasemos a comandos más avanzados```**
  
  **Nota: las importaciones deben de ir hasta arriba, los comandos en el centro y el client.run hasta abajo**
  
```py
#borrar mensajes
@client.command()
async def borrar(ctx, param = 10):
  await ctx.channel.purge(limit = int(param))
```  

```py  
#banear usuarios
@client.command()
async def ban(ctx, member: discord.Member, reason=" "):

    await member.ban(reason=reason)
    await ctx.send("El usuario " + str(member.mention) + " " +
                   "Ha sido sancionado con ban.")
```  
  
```py  
#desbanear usuarios  
@client.command()
async def unban(ctx, *, member):
  banned_users = await ctx.guild.bans()
  member_name, member_discriminator = member.split('#')

  for ban_entry in banned_users:
    user = ban_entry.user

    if(user.name, user.discriminator) == (member_name, member_discriminator):
      await ctx.guild.unban(user)
      await ctx.send(f'{user.mention} ha sido desbaneado')
      return   
```
  
```py
#mostrar la información de una persona
@client.command()
async def userinfo(ctx,user:discord.Member=None):

    if user==None:
        user=ctx.author

    rlist = []
    for role in user.roles:
      if role.name != "@everyone":
        rlist.append(role.mention)

    b = ", ".join(rlist)

    embed = discord.Embed(colour=user.color,timestamp=ctx.message.created_at)

    embed.set_author(name=f"USER INFO - {user}"),
    embed.set_thumbnail(url=user.avatar_url),
    embed.set_footer(text=f'Requested by - {ctx.author}',
  icon_url=ctx.author.avatar_url)

    embed.add_field(name='ID:',value=user.id,inline=False)
    embed.add_field(name='Name:',value=user.display_name,inline=False)

    embed.add_field(name='Bot?',value=user.bot,inline=False)

    embed.add_field(name=f'Roles:({len(rlist)})',value=''.join([b]),inline=False)
```
  
<div align="center">
  <h1> Fin </h1>

**```Si llegaste hasta aquí solo me queda decirte felicidades por crear tu bot y gracias por haber leido la guía completa, me ayudarías mucho dejando una estrella al repositorio y siguiéndome para así poder llegar a más personas, esto es todo.```**
