

!SLIDE
#In what order after_filters will be executed#
    @@@ ruby
    class HomeController < ApplicationController

      before_filter :before_filter1
      before_filter :before_filter2

      after_filter :after_filter1
      after_filter :after_filter2

    end

!SLIDE
# after filters in controller are executed in reverse order #

!SLIDE
# Order of execution #
    @@@ ruby
    before_filter1
    before_filter2
    ...
    after_filter2
    after_filter1

!SLIDE
# ActiveRecord maintains the order of declaration for after_* callbacks#

!SLIDE
    @@@ ruby
    class Record < ActiveRecord::Base

      before_save :before_save1
      before_save :before_save2

      after_save :after_save1
      after_save :after_save2

    end

!SLIDE
# Order matters #

!SLIDE
# ActiveRecord does a lot of work for you through callbacks #

!SLIDE
    @@@ ruby
    class Person < AR::Base
      has_many :children, :dependent => :destroy

      before_destroy :record_number_of_children
    end


!SLIDE
# record_number_of_children will always return zero because by the time method is execute all the children are already deleted #

!SLIDE
#There are many ways to solve the problem. One way is to change the order of callbacks#

!SLIDE
    @@@ ruby
    class Person < AR::Base
      before_destroy :record_number_of_children

      has_many :children, :dependent => :destroy
    end

!SLIDE
#Here the number of children will be reported correctly#
