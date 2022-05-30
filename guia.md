<div align="center">
  <h1> ¡Hola!, bienvenid@ a la guía NO OFICIAL de pycord </h1>
  
  **```Introducción: LA CREACIÓN DEL BOT```**
  
  **```La creación del bot es en realidad bastante sencilla, simplemente tienes que dirigirte a la página de:```** **[Discord Developer Portal](https://discord.com/developers/applications)** 
 
  **```Y seguir los siguientes pasos 👇```**
  
  >**```Paso 1: Ir al apartado de "New Aplication" que se encuentra en la esquina superior derecha y ingresar el nombre del bot, si ya lo ingresaste ¡Muy bien!, acabas de crear tu primer aplicación (si es que no tenías una antes jaja), completados estos pasos vámonos al penúltimo paso. (si nunca haz iniciado sesión en la página de discord developer portal se te pedirá que inicies sesión, solo tienes que ingresar los datos de tu cuenta y listo, ya estás dentro)```**
  
  >**```¡Ya estás muy cerca de crear el bot! tienes que dirigirte al apartado de "Bot" y presionar el botón que dice: add bot, después te aparecerá una ventana que te dirá que la acción de crear un bot es irreversible y bla bla, solamente aprieta el botón de "Yes, do it" y ¡listo! ya tienes creado el bot.```**
  
 > **```Como último paso y el más importante: tienes que dar click en el botón que dice "reset token" y después copiarlo, más adelante veremos para qué funciona el token.```**
  
 **Espero que todo te haya salido bien hasta este momento, si no, déjame un comentario y yo trataré de responder.**
  
  <div align="center">
    <h1> Codificar el bot </h1>
    
**```Bienvenido a ésta nueva sección, aquí encontrarás el código básico para poder encender tu bot y mostrar su latencia, ¿Estás list@? ¡Vamos!```**

> **```Paso 1: Tienes que dirigirte al editor de código de tu elección, también puedes crear un repositorio en una página web como replit o github.```**
    
    
> **```Paso dos: Ya que tu repositorio esté listo tendrás que crear un archivo .py, puedes asignarle el nombre que quieras, pero de preferencia un "main.py"```**
    
    
> **```Paso 3: Ahora tienes que hacer las instalaciones y importar los módulos, abajo te dejaré el código.```**
    
<div align="center">
  <h1> Instalación </h1>
    
```py
#instalar pycord en consola 
pip install pycord
#instalar pycord 2.0 en consola/cmd
py -3 -m pip install -U git+https://github.com/Pycord-Development/pycord
#instalarlo en cmd de macOS
python3 -m pip install -U pycord
#instalarlo en cmd de windows
py -3 -m pip install -U pycord
#instalarlo en cmd de linux
sudo apt-get install pycord
```

<div align="center">
  <h1> Módulos </h1>

```py
import discord
import asyncio
```
  
 **```Nota: discord usa principalmente estos módulos para poder ejecutar correctamente las acciones del bot y no tener ningún error en el código, aclarado esto vamos al siguiente paso```**
  
  <div align="center">
    <h1> Codificando al bot pt1 (código básico) </h1>
  
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
client.run('el token del bot') #encender al bot. Nota: mantener el token como un secreto es muy importante ya que es la llave para poder configuar al bot, así que no se lo compartas a NADIE a menos que sea de mucha confianza, si el token se llega a filtrar resetealo, solo tendrás que ingresar el nuevo token en client.run('token')
```  
  
**```Si todo salió bien hasta aquí: ¡Felicidades!, tu bot ya tiene su primer comando y está encendido, si no, verifica que hayas seguido todos los pasos correctamente```**
    
<div align="center">
  <h1> Codificando al bot pt2 (moderación)</h2>
  
 **```Si llegaste hasta aquí es porque ya tienes listo el código básico, ahora pasemos a comandos de moderación básica```**
  
 **```Nota: las importaciones deben de ir hasta arriba, los comandos en el centro y el client.run hasta abajo```**
  
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
#detectar palabras
@client.event
async def on_message(message):
  channel = message.channel
  if "palabra" in message.content:
    await message.delete()
    await channel.send(f"¡No uses esa palabra {message.author}!")
```
  
```py
#ver mensajes borrados
snipe_message_author = {}
snipe_message_content = {}


@client.event
async def on_message_delete(message):
    snipe_message_author[message.channel.id] = message.author
    snipe_message_content[message.channel.id] = message.content
    await sleep(60)
    del snipe_message_author[message.channel.id]
    del snipe_message_content[message.channel.id]
  
  
@client.command()
async def snipe(ctx):
    channel = ctx.channel
    try:
        snipeEmbed = discord.Embed(
            title=f'Último mensaje borrado en:  #{channel.name}:',
            description=snipe_message_content[channel.id],
            color=(discord.Color.random()))
        snipeEmbed.set_footer(
            text=
            f'El mensaje fué escrito por:  {snipe_message_author[channel.id]}')
        await ctx.send(embed=snipeEmbed)
    except:
        snipeEmbederror = discord.Embed(
            title=f'Error :x:',
            description=f'No hay mensajes borrados en #{channel.mention}',
            color=(discord.Color.random()))
        await ctx.send(embed=snipeEmbederror)
```
  
<div align="center">
  <h1> Codificando al bot pt3 (Entretenimiento) </h1>
  
 
 **```A continuación veremos los comandos de entretenimiento```**
 
```py
#comando 8ball
#antes de hacer este comando tienes que importar el módulo "random"
@client.command(aliases=['8ball', '8b'])
async def bolaocho(ctx, *, question):
    responses = [
        'Demonios, no.', 'Probablemente no.', 'No lo sé bro.', 'Probablemente.', 'Demonios, sí.',
        'Es cierto.', 'Es decididamente así.', 'Sin duda.',
        'Sí - Definitivamente.', 'Cree en ello.', 'Por lo que veo, sí.','Sí!', 'No!', 'Intenta de nuevo.',
        'Mejor no decirlo.', 'No puedo adivinar en este momento.',
        'Concéntrate y pregunta de nuevo.', "No cuentes con eso.", 'Mi respuesta es No.', 'Sigue soñando'
    ]
    embed = discord.Embed(
        title="8ball",
        color=discord.Color.random(),
        description=
        f":8ball: Pregunta: {question}\n:8ball: Respuesta: {random.choice(responses)}"
    )

    await ctx.send(embed=embed)
```

```py
#medir el nivel de homosexualidad
@client.command(aliases=['gayrate'])
async def gay(ctx):
    gayembed = discord.Embed(
        title=':regional_indicator_g: :a: :regional_indicator_y:',
        description=f'Eres **```{random.randrange(101)}```** **%** **GAY**',
        color=discord.Color.random())
    await ctx.send(embed=gayembed)
```

  
<div align="center">
  <h1> Codificando al bot pt4 (comandos extra) </h1>
  
**```A continuación veremos los comandos extra que no tienen una sección en específico```**
  
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
  
```py
#ver la información del servidor
@client.command()
async def serverinfo(ctx):
  embed = discord.Embed(title = f"{ctx.guild.icon}")
  embed.add_field(name = "Nombre", value = f"{ctx.guild.name}")
  embed.add_field(name = "ID", value = f"{ctx.guild.id}")
  embed.add_field(name = "Región", value = f"{ctx.guild.region}")
  embed.add_field(name = "Members", value = f"{ctx.guild.members}")
  embed.add_field(name = "Roles", value = len(ctx.guild.roles))
  embed.add_field(name = "Canales", value = f"{ctx.guild.text_channels}"
  embed.add_field(name = "Canales de voz", value = f"{ctx.guild.voice_channels}")
  embed.add_field(name = "Owner", value = f"{ctx.guild.owner}")

  await ctx.reply(embed = embed) 
```
  
```py
#mostrar el avatar de una persona
@client.command(name='avatar')
async def avatar(ctx, user: discord.Member):
    mbed = discord.Embed(color=discord.Color.random(), title=f"{user}")
    mbed.set_image(url=f'{user.avatar_url}')
    mbed.set_footer(text=f'Sugerido por: {ctx.author}',
                    icon_url=ctx.author.avatar_url)
    await ctx.send(embed=mbed)


@avatar.error
async def av_error(ctx, error):
    if isinstance(error, commands.MissingRequiredArgument):
        mbed = discord.Embed(color=discord.Color.random(),
                             title=f"{ctx.author}")
    mbed.set_image(url=f'{ctx.author.avatar_url}')
    await ctx.send(embed=mbed)
```
 
     
<div align="center">
  <h1> Fin </h1>

**```Si llegaste hasta aquí solo me queda decirte felicidades por crear tu bot y gracias por haber leido la guía completa, me ayudarías mucho dejando una estrella al repositorio y siguiéndome para así poder llegar a más personas, esto es todo.```**
  
<div align="center">
  <h1> docs </h1>
<div>
 
# [Documentación de Pycord](https://docs.pycord.dev/en/master/)  
# [Documentación de Discord](https://discord.com/developers/docs/intro)
