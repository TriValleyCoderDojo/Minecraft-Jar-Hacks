Minecraft-Jar-Hacks
===================

A collection of Minecraft Jar Hacks:

1) change TNT, fuse and explosion



2) make a SnowBall deadly when thrown

open the net.minecraft.entity.projectile.EntitySnowball class
find the onImpact() method
change the 
line byte b0 = 0;
to be
byte b0 = 127;

3) make a SnowBall explode on impact when thrown

open the net.minecraft.entity.item.EntityTNTPrimed class
find and copy the explode() method
open the net.minecraft.entity.projectile.EntitySnowball class
set the value of b0 back to 0 if you did hack #2
paste the explode() method into the EntitySnowball class
find the onImpact() method
add a call to the explode() method just before setDead()
        if (!this.worldObj.isRemote)
        {
        	this.setDead();
        }
will become
        if (!this.worldObj.isRemote)
        {
        	this.setDead();
        }

4) make a Bucket throw an entity in the air when hit 

open the net.minecraft.item.ItemBucket 
add the method to the class
	public boolean hitEntity(ItemStack itemstack, EntityLivingBase target, EntityLivingBase player){
		if(!target.worldObj.isRemote){
			target.motionY = 2.5;
			return true;
		}
		return false;
	}

5) make TNT throw WitherSkulls with right click

6) change the on-death drops

