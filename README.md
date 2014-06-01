Minecraft-Jar-Hacks
===================

This is a collection Jar Hacks for Minecraft.  You can follow these steps to create some simple Jar Hacks and get started with customizing Minecraft.  These will need to be done inside of the Forge Mod Loader 1.6.4.

Note: These are intended to be used in a local environment only, and for the purpose of learning only.  In general, Jar Hacks are frowned upon and are not maintainable. Never attempt to distribute a Jar Hack. 

#Additional Information:
- **Setup Java, Eclipse & FML**, helpful hints and tips on the [Wiki](https://github.com/TriValleyCoderDojo/Minecraft-Jar-Hacks/wiki)
- **Make a deployable Mod**, try the [RedDiamond Tutorial](http://www.lopakalogic.com/articles/minecraft-articles/minecraft-mods-forge)

## Change TNT, fuse and explosion

###To change the fuse length
- Open the net.minecraft.entity.item.EntityTNTPrimed class
- Find the line where the fuse time gets set

```code
this.fuse = 80;
```

- You may then change this number to whatever you would like to increase or decrease the time it take for TNT to explode.
- Restart the server and try it out

###To change the size of the explosion

- Stop the currently running server
- Open the net.minecraft.entity.item.EntityTNTPrimed class
- Find the explode() method and the variable f

```code
float f = 4.0F;
```

- You may then change this number to whatever you would like to increase or decrease the time it take for TNT to explode.  Although you may not want to make it too large, because it will cause lag.  
- Restart the server and try it out


## Make a SnowBall deadly when thrown

- Stop the currently running server
- Open the net.minecraft.entity.projectile.EntitySnowball class
- Find the onImpact() method
- Change the line 
   
```code
     byte b0 = 0;
```
   
   to be
   
```code
     byte b0 = 127;
```

- Restart the server and try it out

## Make a SnowBall explode on impact when thrown


- Stop the currently running server
- Open the net.minecraft.entity.item.EntityTNTPrimed class
- Find and copy the explode() method
- Open the net.minecraft.entity.projectile.EntitySnowball class
- Set the value of b0 back to 0 if you did hack #2
- Paste the explode() method into the EntitySnowball class
- Find the onImpact() method
- Add a call to the explode() method just before setDead()
  
```code
        if (!this.worldObj.isRemote) {
        	this.setDead();
        }
```
  
will become

```code
        if (!this.worldObj.isRemote){
        	this.explode();
        	this.setDead();
        }
```

- Restart the server and try it out

## Make a Bucket throw an entity in the air when hit 

- Stop the currently running server
- Open the net.minecraft.item.ItemBucket class
- Add the method to the class

```code
	public boolean hitEntity(ItemStack itemstack, EntityLivingBase target, EntityLivingBase player){
		if(!target.worldObj.isRemote){
			target.motionY = 2.5;
			return true;
		}
		return false;
	}
```
In case Eclipse does not automatically add the import statement for EntityLivingBase, you may need to manually add it.

```code
import net.minecraft.entity.EntityLivingBase;
```

- Restart the server and try it out

## Make a Shovel (any of them) throw WitherSkulls when right clicked

- Stop the currently running server
- Open the net.minecraft.item.ItemSpade class
- Add the method to the class

```code
	public ItemStack onItemRightClick(ItemStack itemstack, World world, EntityPlayer entityplayer) {
		itemstack.damageItem(10, entityplayer);
		if (!world.isRemote) {
			Vec3 look = entityplayer.getLookVec();
			EntityWitherSkull witherSkull = new EntityWitherSkull(world, entityplayer, 1, 1, 1);
			witherSkull.setPosition(
					entityplayer.posX + look.xCoord * 5,
					entityplayer.posY + look.yCoord * 5,
					entityplayer.posZ + look.zCoord * 5);
			witherSkull.accelerationX = look.xCoord * 0.1;
			witherSkull.accelerationY = look.yCoord * 0.1;
			witherSkull.accelerationZ = look.zCoord * 0.1;
			world.spawnEntityInWorld(witherSkull);
		}
		return itemstack;
	}
```

In case Eclipse does not automatically add the import statements for the new classes, you may need to manually add them.

```code
import net.minecraft.entity.player.EntityPlayer;
import net.minecraft.entity.projectile.EntityWitherSkull;
import net.minecraft.util.Vec3;
import net.minecraft.world.World;
```

- Restart the server and try it out

## Make zombie drop something other than rotten flesh

- Stop the currently running server
- Open the net.minecraft.entity.monster.EntityZombie class
- Find the method getDropItemId()
- Change the return itemId
- Restart the server and try it out

For example:

```code
return Item.axeDiamond.itemID;
```

would make the Zombie drop diamond axes.

Note: Please becareful to make sure you return a valid Item ID. 


## Change the extra on-death drops and drop amounts

- Stop the currently running server
- Under the net.minecraft.entity.monster package, you will find a number of entities, one of: 
 EntityBlaze, EntityEnderman, EntityGhast, EntityIronGolem, EntityMagmaCube, EntityPigZombie, EntitySkeleton, EntitySnowman, EntitySpider, EntityWitch
- Open the class for the entity you wish to edit
- Find the dropFewItems() method, controls additionals drops and amounts of those drops
- Look for the this.dropItem() method.
- Change the item specified in the function
- Or you can change the number of times it loops by changing i & k
- Restart the server and try it out


## Change the sound of a monster when it dies

- Stop the currently running server
- Open class net.minecraft.entity.monster.EntityWitch or whatever monster
- Find the method getDeathSound()
- Change the mapping to the ogg file for a different sound
- Restart the server and try it out

For example, changing 

```code
return "mob.witch.death";
```

to 

```code
return "mob.ghast.death";
```

would make the Witch sound like the Ghast.  

The sounds are mapped to ogg files, and you can find them in the directory mcp\jars\assets\sound.  In case of the ghast there is a mob\ghast directory with a death.ogg file.  However, only other death sounds seem to work.  

## Make snow golems leave a trail of snow (or ice) wherever they go

- Stop the currently running server
- Open class et.minecraft.entity.monster.EntitySnowman
- Find the method onLivingUpdate()
- Scroll down to line 65, which will look like:
```code
if (this.worldObj.getBiomeGenForCoords(i, j).getFloatTemperature() > 1.0F)
```
- Change 1.0F to 100.0F so the snow golem can live in hot biomes
- Scroll down to line 76, which will look like:
```code
if (this.worldObj.getBlockId(j, k, l) == 0 && this.worldObj.getBiomeGenForCoords(j, l).getFloatTemperature() < 0.8F && Block.snow.canPlaceBlockAt(this.worldObj, j, k, l))
```
- This line is checking the current temperature, change the 0.8F to 100.0F, which increases how it can be to place snow
- Restart the server and try it out

Note: You can change the kind of trail from snow to ice by changing
- On line 76 Block.snow.canPlaceBlockAt to Block.ice.canPlaceBlockAt
- On line 78 Block.snow.blockID to Block.ice.blockID

### Create a snow golem

- You will need two blocks of Snow, and one Pumpkin
- Stack the two Snow blocks on top of each other, and place the Pumpkin on the top
- This will spawn a Snow golem

Source: Arun Gupta

## Change the max enchantment level of an item

Normally, when you attempt to enchant an item that you are holding in the hotbar with the command:

/enhanct <player> <enchantment_id> <level>

you will have a limit on the maximum level that can be used (eg: 2). You can change the level to something
higher if you would like.  

- Stop Minecraft.
- Open class net.minecraft.enchantment.Enchantment
- At the top of this class all of the different enchantments are created
- We will change the level for KnockBack, which has an id of 19, see line 53
- Open the EnchantmentKnockback class
- Find the method getMaxLevel()
- Change the default number (2) in return statement to something else, like 10.
- Restart the server and you will be able to use a level of 10 for KnockBack

Warning: Some enchantments(looting and efficiency) cause lag at high levels. I would reccommend against levels above 50.

Source: Arun Gupta

## Add a new furnace recipe

- Stop the currently running server
- Open class net.minecraft.item.crafting.FurnaceRecipes
- Find the private constructor, private FurnaceRecipes()
- Add a new recipe, the parameters are:
   1. item to put into the furnace
   2. item to get from smelting
   3. experience points to get
- Restart the server and try it out

For example: 

```code
this.addSmelting(Item.ingotIron.itemID, new ItemStack(Item.ingotGold), 1.0F);
```

would smelt a gold ingot from an iron ingot.  

Note: Please becareful to avoid repeating the first item more than once, because it will confuse Minecraft. 


## Make a Chicken Lay Diamonds Really Fast

- Stop the currently running server
- Open class the net.minecraft.entity.passive.EntityChicken
- Find the method onLivingUpdate()
- Change the dropped item
- Find everywhere the variable timeUntilNextEgg gets
- Change the to make it more often
- Restart the server and try it out

For example, changing 

```code
this.dropItem(Item.egg.itemID, 1);
...
this.timeUntilNextEgg = this.rand.nextInt(6000) + 6000;
```

to 

```code
this.dropItem(Item.diamond.itemID, 1);
...
this.timeUntilNextEgg = this.rand.nextInt(6) + 6;
```

would make the chickens lay diamonds instead of eggs, and they will come out in almost a stream.  

**Note:** the `timeUntilNextEgg` needs to be changed in two places: once at the top of the class in the EntityChicken() constructor and again in the onLivingUpdate() method near the bottom.  The first one is used for the intial time it takes for the chicken to start laying diamonds, and the second one is for all the times after that.  

## Make arrows explode when they hit an entity

- Stop the currently running server
- Open the net.minecraft.entity.projectile.EntityArrow class
- Find the method onUpdate(), about line 186
- Find the place in the code where the damage gets inflicted, about line 360 looks like:
```code
this.playSound("random.bowhit", 1.0F, 1.2F / (this.rand.nextFloat() * 0.2F + 0.9F));
```
- Insert the following line after the above to cause an explosion
```code
this.worldObj.createExplosion(this, this.posX, this.posY, this.posZ, 2.0F, true);
```
- Restart the server and try it out

Note: You can experiment with the size of the explosiion by change the value of 2.0F to some other number

