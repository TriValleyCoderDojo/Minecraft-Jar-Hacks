Minecraft-Jar-Hacks
===================

This is a collection Jar Hacks for Minecraft.  You can follow these steps to create some simple Jar Hacks and get started with customizing Minecraft.  These will need to be done inside of the Forge Mod Loader.

Note: These are intended to be used in a local environment only, and for the purpose of learning only.  In general, Jar Hacks are frowned upon and are not maintainable. Never attempt to distribute a Jar Hack. 

Additional information is available on the [wiki](https://github.com/TriValleyCoderDojo/Minecraft-Jar-Hacks/wiki)

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


## Change the on-death drops

- Stop the currently running server
- Under the net.minecraft.entity.monster package, you will find a number of entities
- Open the class for the entity you wish to edit
- Find the dropFewItems() method

```code

    protected void dropFewItems(boolean par1, int par2) {
        int j = this.rand.nextInt(3);
        int k;
        for (k = 0; k < j; ++k) {
            this.dropItem(Block.plantRed.blockID, 1);
        }
        k = 3 + this.rand.nextInt(3);
        for (int l = 0; l < k; ++l) {
            this.dropItem(Item.ingotIron.itemID, 1);
        }
    }
```

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


## Change the max enchantment level of an item


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

