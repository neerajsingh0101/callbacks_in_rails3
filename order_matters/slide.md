!SLIDE
# Order matters #

!SLIDE
# after filters in controller are executed in reverse order #

!SLIDE

    @@@ ruby
    class HomeController < ApplicationController

      before_filter :before_filter1
      before_filter :before_filter2

      after_filter :after_filter1
      after_filter :after_filter2

    end

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

# ActiveRecord does a lot of work for you through callbacks #

!SLIDE

    @@@ ruby
    class Person < AR::Base
      has_many :children, :dependent => :destroy

      before_destroy :record_number_of_children
    end


!SLIDE
# record_number_of_children will always return zero because by the time method is execute all the children are already deleted #
