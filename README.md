CakePHP Notification Plugin
===========================

CakePHP Notification Plugin provide an notification system

Installation 
------------------------------

Download the plugin

	cd NameProjects/app
	git clone git://github.com/aschelch/cakephp-notification-plugin.git Plugin/Notification


Attach the Notifiable behavior to the user model

	class User extends AppModel {
		public $actsAs = [
			'Notification.Notifiable' => [
				'subjects' => ['User', 'Post', 'Comment']
			]
		];
	}



Usage
------------------------------

Notify the user #1 that the user #2 posted a comment (#123) on user #1's post (#456)

	$this->User->notify(1, 'post_comment', ['User'=>2,'Post'=>456,'Comment'=>123]);

Create an element in Elements/Notification/post_comment.ctp

	<a href="<?= $this->Html->url(array('controller'=>'post','action'=>'view',$data['Post']['id'])) ?>">
		<?= __("%s comment your post %s", '<strong>'.$data['User']['username'].'</strong>', '<strong>'.$data['Post']['title'].'</strong>'); ?><br/>
		<small><?= $this->Time->niceShort($data['Notification']['created']); ?></small>
	</a>

In your layout, display the notifications

	<ul class="nav pull-right">
		<?php $notifications = ClassRegistry::init('User')->getUnreadNotification(AuthComponent::user('id')); ?>
		<li class="dropdown">
			<a href="#" class="dropdown-toggle" data-toggle="dropdown">
				<?= __('Notifications') ?>
				<?php if (!empty($notifications)): ?>
					<span class="badge badge-success">
						<?php echo count($notifications); ?>
					</span>
				<?php endif ?>
			</a>
			<ul class="dropdown-menu">
				<?php foreach ($notifications as $notification): ?>
					<li><?= $this->Notification->display($notification); ?></li>
					<li class="divider"></li>
				<?php endforeach ?>
				<li class="text-center"><?= $this->Html->link(__('Display all'), '#'); ?></li>
			</ul>
		</li>
	</ul>




