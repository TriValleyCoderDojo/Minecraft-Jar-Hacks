Minecraft-Jar-Hacks
===================

This is a collection Jar Hacks for Minecraft.  You can follow these steps to create some simple Jar Hacks and get started with customizing Minecraft.  These will need to be done inside of the Forge Mod Loader.

Note: These are intended to be used in a local environment only, and for the purpose of learning only.  In general, Jar Hacks are frowned upon and are not maintainable. Never attempt to distribnute a Jar Hack. 

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
In case Eclipse does not automatically add the import statement for EntityLivingBase, you may need to manually add it

```code
import net.minecraft.entity.EntityLivingBase;
```

- Restart the server and try it out

## Make a Shovel (any of them) throw WitherSkulls with right clicked

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

- Restart the server and try it out

## Change the on-death drops

- Stop the currently running server
- Find the entity you wish to edit
- Find the dropFewItems class
- In that class their will be a return statement(the example is an Iron Golem)

```code

protected void dropFewItems(boolean par1, int par2)
    {
        int j = this.rand.nextInt(3);
        int k;

        for (k = 0; k < j; ++k)
        {
            this.dropItem(Block.plantRed.blockID, 1);
        }

        k = 3 + this.rand.nextInt(3);

        for (int l = 0; l < k; ++l)
        {
            this.dropItem(Item.ingotIron.itemID, 1);
        }
    }
```

- Look for the this.dropItem() method.
- Change the item specified in the function
- Restart the server and try it out
