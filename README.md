Minecraft-Jar-Hacks
===================

A collection of Minecraft Jar Hacks:

1) change TNT, fuse and explosion



2) make a SnowBall deadly when thrown

- open the net.minecraft.entity.projectile.EntitySnowball class
- find the onImpact() method
- change the line 
```code
     byte b0 = 0;
```
   to be
```code
     byte b0 = 127;
```

3) make a SnowBall explode on impact when thrown

- open the net.minecraft.entity.item.EntityTNTPrimed class

- find and copy the explode() method

- open the net.minecraft.entity.projectile.EntitySnowball class

- set the value of b0 back to 0 if you did hack #2

- paste the explode() method into the EntitySnowball class

- find the onImpact() method

- add a call to the explode() method just before setDead()
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

4) make a Bucket throw an entity in the air when hit 

- open the net.minecraft.item.ItemBucket 

- add the method to the class
```code
	public boolean hitEntity(ItemStack itemstack, EntityLivingBase target, EntityLivingBase player){
		if(!target.worldObj.isRemote){
			target.motionY = 2.5;
			return true;
		}
		return false;
	}
```

5) make TNT throw WitherSkulls with right click

6) change the on-death drops

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
	-Change the item specified in the function
